<template>
  <header>
         <div id="logo">
            <a href="/"><img src="../assets/artd-logo.svg" alt="Artd Logo" width="68" height="32" style="filter: invert(10%) sepia(13%) saturate(431%) hue-rotate(202deg) brightness(11%) contrast(89%);"></a>
         </div>
         <div id="burger" class="burger-menu">
            <span></span>
            <span></span>
            <span></span>
          </div>
         <div id="menu">
            <ul>
               <li><a href="https://compressor.artd.ch/">Compressor</a></li>
               <li><a href="/">BG-Removal</a></li>
            </ul>
         </div>
      </header>
</template>

<script>
export default {
  name: 'BgRemoveNavBar',
  mounted() {
    const burger = document.getElementById('burger');
    const menu = document.getElementById('menu');
    
    burger.addEventListener('click', () => {
      burger.classList.toggle('active');
      menu.classList.toggle('active');
    });
  },
};
</script>

<style scoped>
header {
    width: 80%;
    max-width: 1200px;
    min-width: 1200px;
    margin: 20px auto;
    padding: 20px;
    height: 30px;
    position: relative;
    z-index: 4;
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
}
#logo {
    font-weight: 800;
    color: #222;
    float: left;
}
#menu {
    width: auto;
    float: right;
}
#menu ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
    text-align: right;
    display: flex;
    list-style: none;
    gap: 15px;
}
#burger {
          display: none;
          flex-direction: column;
          cursor: pointer;
          gap: 5px;
          position: absolute;
          right: 25px; /* Align to the right */
        }
        
        #burger span {
          width: 25px;
          height: 3px;
          background-color: #000;
          transition: all 0.3s ease-in-out;
        }
        #burger.active span:nth-child(1) {
          transform: rotate(45deg) translate(5px, 6.5px);
        }
        
        #burger.active span:nth-child(2) {
          opacity: 0;
        }
        
        #burger.active span:nth-child(3) {
          transform: rotate(-45deg) translate(5px, -6.5px);
        }
#menu ul li {
    display: inline-block;
}
#menu ul li a {
    padding: 4px 10px;
    margin-left: 10px;
    text-decoration: none;
    color: #435775;
    font-weight: 600;
    font-size: 14px;
    transition: all .12s linear;
}
#menu ul li a:hover {
    color: #A8D45A
}
@media screen and (max-width: 1261px) {
    header {
        min-width:unset;
        width: 92%
    }
}

@media screen and (max-width: 570px) {
    header {
        width:98%;
        padding: 10px;
        box-sizing: border-box
    }

    #menu {
        margin-right: 15px;
        float: right;
        width: 140px;
        padding-top: 10px
    }

    #menu ul li a {
        font-size: 11px
    }

    #logo {
        font-size: 14px
    }
}
@media (max-width: 768px) {
          #menu ul {
            display: none;
            flex-direction: column;
            position: absolute;
            top: 60px;
            right: 10px;
            background-color: #fff;
            padding: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
          }
        
          #burger {
            display: flex;
          }
        
          #menu.active ul {
            display: flex;
          }
        }
</style>
