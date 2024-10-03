<template>
    <div class="container">
      <div
        class="upload-box"
        :class="{ dragover: isDragging }"
        @dragover.prevent="onDragOver"
        @dragleave="onDragLeave"
        @drop.prevent="onDrop"
      >
        <input
          type="file"
          id="file-upload"
          @change="onFileChange"
          accept="image/*"
          multiple
          hidden
        />
        <label for="file-upload" class="upload-label">
          <div class="upload-content">
            <p>Bilder ziehen und ablegen</p>
            <p>oder <span class="browse-text">zum Hochladen durchsuchen.</span>.</p>
            <button class="upload-button" @click="triggerFileUpload">
              <i class="fas fa-upload"></i> Fotos hochladen
            </button>
          </div>
        </label>
      </div>
      <div v-if="isLoading" class="loading">
        <p>Processing...</p>
        <div class="spinner"></div>
      </div>
      <div v-if="images.length > 0" class="gallery-container">
        <div class="thumbnail-gallery">
          <div
            v-for="(image, index) in images"
            :key="index"
            class="thumbnail"
            :class="{ active: selectedImage === index }"
            @click="selectImage(index)"
          >
            <img :src="image.imageUrl" alt="Thumbnail" />
          </div>
        </div>
        <div v-if="selectedImage !== null" class="main-image">
          <img :src="images[selectedImage].resultUrl" alt="Main Display" class="main-display-image" />
          <div class="image-actions">
            <a :href="images[selectedImage].resultUrl" download="processed-image.png" class="icon-button download-button">
              <i class="fas fa-download"></i>
            </a>
            <button class="icon-button remove-button" @click="removeImage(selectedImage)">
              <i class="fas fa-times"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { removeBackground } from "@imgly/background-removal";
  
  export default {
    data() {
      return {
        images: [],
        selectedImage: null,
        isLoading: false, // Track loading state
        isDragging: false, // Track dragging state
      };
    },
    methods: {
      triggerFileUpload() {
        document.getElementById("file-upload").click();
      },
      onFileChange(event) {
        const files = Array.from(event.target.files);
        this.handleFiles(files);
      },
      onDragOver() {
        this.isDragging = true;
      },
      onDragLeave() {
        this.isDragging = false;
      },
      onDrop(event) {
        this.isDragging = false;
        const files = Array.from(event.dataTransfer.files);
        this.handleFiles(files);
      },
      async handleFiles(files) {
        // Start loading when files are dropped or selected
        this.isLoading = true;
        
        // Process each file one by one using async/await
        for (let file of files) {
          await this.processSingleFile(file);  // Ensure each image is processed before moving to the next
        }

        // Once all files are processed
        this.isLoading = false;
        console.log("All images have been processed.");
      },
      async processSingleFile(file) {
        const image = {
          file: file,
          imageUrl: URL.createObjectURL(file),
          resultUrl: null
        };
        
        // Add image to the list before processing
        this.images.push(image);
        
        try {
          console.log("Starting background removal for an image...");
          const config = {
            debug: true,
            device: 'gpu',
            model: 'medium',
            output: {
              format: 'image/png',
              quality: 1,
              type: 'background',
            },
          };
          
          const blob = await removeBackground(file, config);
          image.resultUrl = URL.createObjectURL(blob);
          console.log("Background removal completed for the image.");
          
          // Set the first image as selected by default
          if (this.selectedImage === null) {
            this.selectedImage = this.images.length - 1;
          }
        } catch (error) {
          console.error("Background removal failed for an image:", error);
        }
      },
      selectImage(index) {
        this.selectedImage = index;
      },
      removeImage(index) {
        this.images.splice(index, 1);
        if (this.selectedImage === index) {
          this.selectedImage = this.images.length > 0 ? 0 : null;
        } else if (this.selectedImage > index) {
          this.selectedImage--;
        }
      }
    },
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 1200px;
    padding: 20px;
  }
  
  .upload-box {
    border: 2px dashed #ccc;
    border-radius: 12px;
    padding: 30px;
    margin-top: 20px;
    background-color: #f9f9f9;
    transition: background-color 0.3s ease;
  }
  
  .upload-box.dragover {
    background-color: #e6f7ff;
  }
  
  .upload-label {
    display: block;
    cursor: pointer;
  }
  
  .upload-content {
    text-align: center;
  }
  
  .upload-content p {
    margin: 0;
  }
  
  .browse-text {
    color: #A8D45A;
    cursor: pointer;
  }
  
  .upload-button {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 20px auto;
    padding: 10px 20px;
    font-size: 16px;
    color: #fff;
    background-color: #A8D45A;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .upload-button i {
    margin-right: 8px;
  }
  
  .upload-button:hover {
    background-color: #779541;
  }
  
  .loading {
    margin: 20px 0;
    text-align: center;
  }
  
  .spinner {
    width: 40px;
    height: 40px;
    margin: 0 auto;
    border: 4px solid rgba(0, 0, 0, 0.1);
    border-left-color: #000;
    border-radius: 50%;
    animation: spin 1s linear infinite;
  }
  
  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
  
  .gallery-container {
    display: flex;
    flex-direction: column;
    margin-top: 20px;
  }
  
  .thumbnail-gallery {
    display: flex;
    overflow-x: auto;
    margin-bottom: 20px;
  }
  
  .thumbnail {
    flex: none;
    margin-right: 10px;
    cursor: pointer;
    border: 2px solid transparent;
    transition: border 0.3s ease;
    position: relative;
  }
  
  .thumbnail img {
    width: 60px;
    height: 60px;
    object-fit: cover;
    border-radius: 8px;
  }
  
  .thumbnail:hover::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(26, 26, 26, 0.1);
    border-radius: 8px;
  }
  
  .thumbnail.active {
    border-color: #007bff;
    border-radius: 10px;
  }
  
  .main-image {
    position: relative;
    background-color: #f0f0f0;
    background-image: linear-gradient(45deg, #ddd 25%, transparent 25%), 
                      linear-gradient(135deg, #ddd 25%, transparent 25%), 
                      linear-gradient(45deg, transparent 75%, #ddd 75%), 
                      linear-gradient(135deg, transparent 75%, #ddd 75%);
    background-size: 20px 20px; /* Adjust the size as needed */
    background-position: 0 0, 10px 0, 10px -10px, 0px 10px;
    padding: 10px;
    border-radius: 12px;
    text-align: center;
  }
  
  .main-display-image {
    max-width: 100%;
    max-height: 50vh; /* Ensures the image doesn't exceed the viewport height */
    object-fit: contain; /* Ensures the image maintains its aspect ratio */
    border-radius: 12px;
  }
  
  .image-actions {
    position: absolute;
    top: 10px;
    right: 10px;
    display: flex;
    gap: 10px;
  }
  
  .icon-button {
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #007bff;
    border: none;
    border-radius: 50%;
    color: white;
    font-size: 20px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .icon-button i {
    margin: 0;
  }
  
  .icon-button:hover {
    background-color: #0056b3;
  }
  
  .download-button {
    background-color: #28a745;
  }
  
  .download-button:hover {
    background-color: #218838;
  }
  
  .remove-button {
    background-color: #dc3545;
  }
  
  .remove-button:hover {
    background-color: #c82333;
  }
  </style>
  