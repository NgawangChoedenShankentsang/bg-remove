# Background Removal Tool

This is a Vue.js-based web application that allows users to remove backgrounds from images directly in the browser. The application utilizes the `@imgly/background-removal` package to perform background removal efficiently.

## Features

- **In-Browser Background Removal**: Remove image backgrounds directly in the browser, ensuring data privacy.
- **Multiple Image Upload**: Upload multiple images at once and process them individually.
- **Thumbnail Gallery**: Navigate through uploaded images using a horizontal thumbnail gallery.
- **Checkerboard Background**: Display a checkerboard pattern behind the processed images for better visualization of transparency.
- **Download and Remove Options**: Download processed images and remove them from the gallery.

## Technologies Used

- Vue.js
- @imgly/background-removal
- HTML5 and CSS3

## Getting Started

Follow these instructions to get a copy of the project up and running on your local machine.

### Prerequisites

- Node.js and npm installed on your machine.

### Installation

1. **Clone the repository:**

    ```bash
    git clone https://github.com/NgawangChoedenShankentsang/bg-remove.git
    ```

2. **Navigate to the project directory:**

    ```bash
    cd bg-remove
    ```

3. **Install the dependencies:**

    ```bash
    npm install
    ```

### Running the Application

1. **Start the development server:**

    ```bash
    npm run serve
    ```

2. **Open your browser and navigate to:**

    ```
    http://localhost:8080
    ```

### Usage

1. **Upload Images**: Click on the upload button or drag and drop images into the designated area.
2. **View Thumbnails**: Thumbnails of the uploaded images will appear in a horizontal gallery.
3. **Select and View Processed Image**: Click on a thumbnail to view the processed image with the background removed.
4. **Download Processed Image**: Click the download button to save the processed image.
5. **Remove Image**: Click the remove button to delete the image from the gallery.

## Contributing

Feel free to contribute to this project by submitting a pull request.

