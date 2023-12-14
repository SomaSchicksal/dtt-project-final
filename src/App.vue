<template>
  <div class="app-container">
    <nav v-if="isLargeScreen" class="nav">
      <img :src="logo" class="logo">
      <router-link to="/" class="nav-link">Home</router-link>
      <router-link to="/favoritesList" class="nav-link">Favorites</router-link>
      <router-link to="/about" class="nav-link">About</router-link>
    </nav>
    <!-- This component renders the whole app, starting from its nav if the resolution
    is big enough -->

    <div v-else class="nav2">
      <router-link to="/">
        <img :src="activeHome" v-if="isActive('/')" class="nav-icon">
        <img :src="home" v-else class="nav-icon">
      </router-link>

      <router-link to="/about">
        <img :src="activeAbout" v-if="isActive('/about')" class="nav-icon" >
        <img :src="about" v-else class="nav-icon">
      </router-link>

      <router-link to="/favoritesList">
        <img :src="like" class="nav-icon" v-if="isActive('/favoritesList')">
        <img :src="unlike" class="nav-icon" v-else>
      </router-link>
    </div>

    <!-- if the resolution is low this div will be rendered instead (which is the mobile nav) -->
    <router-view />
  </div>
</template>

<script>
import Logo from "../src/assets/img_logo_dtt@3x.png";
import ActiveHome from "../src/assets/ic_mobile_navigarion_home_active@3x.png";
import Home from "../src/assets/ic_mobile_navigarion_home@3x.png";
import ActiveAbout from "../src/assets/ic_mobile_navigarion_info_active@3x.png";
import About from "../src/assets/ic_mobile_navigarion_info@3x.png";
import Like from "../src/assets/like.jpg";
import Unlike from "../src/assets/unlike.png";

export default {
  data() {
    return {
      logo: Logo,
      activeHome: ActiveHome,
      home: Home,
      activeAbout: ActiveAbout,
      about: About,
      like: Like,
      unlike: Unlike,
      isLargeScreen: true
    }
  },

  methods: {
    handleWindowSize() {
      // Update isLargeScreen based on window size
      this.isLargeScreen = window.innerWidth > 1000; 
    },

    isActive(route) {
      return this.$route.path === route;
    },
  },
  mounted() {
    
    this.handleWindowSize();
    // Sets initial screen size

    
    window.addEventListener('resize', this.handleWindowSize);
    // Adds event listener for window resize
  },
  beforeDestroy() {
    
    window.removeEventListener('resize', this.handleWindowSize);
    // Remove event listener to prevent memory leaks
  },
}
</script>

<style>
/* ==========================================================================
   App Component
   ========================================================================== */

/* Sub-section: Root
   ========================================================================== */

/* CSS variables Style
   ========================================================================== */

:root {
  --element-primary-color: #EB5440;
  --element-secondary-color: #4A4B4C;
  --element-tertiary-color1: #E8E8E8;
  --element-tertiary-color2: #C3C3C3;
  --element-background-color1: #F6F6F6;
  --element-background-color2: #FFFFFF;

  --text-primary-color: #000000;
  --text-secondary-color: #4A4B4C;
  --text-hyperlink-color: #067BC2;
  --font-monserrat: 'Montserrat';
  --font-open-sans: "Open Sans";

  --font-montserrat-bold: 'Montserrat Bold', sans-serif;
  --font-montserrat-semibold: 'Montserrat Semibold', sans-serif;
  --font-montserrat-medium: 'Montserrat Medium', sans-serif;
  --font-montserrat-medium-italic: 'Montserrat Medium Italic', sans-serif;

  --font-open-sans-semibold: 'Open Sans Semibold', sans-serif;
  --font-open-sans-regular: 'Open Sans Regular', sans-serif;

  --font-size-h1-desktop: 32px;
  --font-size-h1-mobile: 18px;
  --font-size-back-button-label-desktop: 16px;
  --font-size-desktop-header-inactive-menu: 18px;
  --font-size-empty-state-message-desktop: 18px;
  --font-size-empty-state-message-mobile: 14px;
  --font-size-input-field-title-desktop: 14px;
  --font-size-input-field-title-mobile: 12px;
  --font-size-h2-desktop: 22px;
  --font-size-h2-mobile: 14px;
  --font-size-desktop-header-active-menu: 18px;
  --font-size-buttons-and-tabs-desktop: 18px;
  --font-size-buttons-and-tabs-mobile: 12px;
  --font-size-error-message-desktop: 14px;
  --font-size-error-message-mobile: 12px;

  --font-size-listing-information-desktop: 16px;
  --font-size-listing-information-mobile: 12px;

  --font-size-body-text-desktop: 18px;
  --font-size-body-text-mobile: 12px;

  --font-size-input-field-desktop: 14px;
  --font-size-input-field-mobile: 12px;

  --font-weight-bold: bold;
  --font-weight-semibold: 600;
  --font-weight-medium: 500;
}

/* box-sizing Style
   ========================================================================== */

* {
  box-sizing: border-box;
}

/* body and html Style
   ========================================================================== */

body,
html {
  margin: 0;
  padding: 0;
}

/* Sub-section: App
   ========================================================================== */

/* App Style
   ========================================================================== */

#app {

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  min-height: 100vh;
  color: var(--text-primary-color);




}

/* H1 in app Style
   ========================================================================== */

#app h1 {
  font-family: "Montserrat", sans-serif;
  font-weight: 700;



  font-size: var(--font-size-h1-desktop);
  color: var(--text-primary-color);
}

/* H2 in App Style
   ========================================================================== */

#app h2 {
  font-family: "Montserrat", sans-serif;
  font-weight: 600;
  font-size: var(--font-size-h2-desktop);
  color: var(--text-primary-color);
}

/* Button in App Style
   ========================================================================== */

.button {

  font-family: var(--font-montserrat-bold);
  font-weight: var(--font-weight-bold);
  font-size: var(--font-size-buttons-and-tabs-desktop);
  color: white;
  padding: 0.5em 3em 0.5em 3em;
  border: none;
  transition: background-color 0.3s ease;
  border-radius: 5px;
  background-color: var(--element-primary-color);
}

.button:hover {
  background-color: rgba(235, 84, 64, 0.7);
}

/* Nav Style
   ========================================================================== */

nav {
  padding: 30px;
}

nav a {
  font-family: "Open Sans", sans-serif;
  font-weight: 500;
  color: var(--element-tertiary-color2);
}

nav a.router-link-exact-active {
  font-family: "Montserrat", sans-serif;
  font-weight: 700;
  font-size: var(--font-size-desktop-header-active-menu);
  color: var(--text-primary-color);
}

.nav-link {
  text-decoration: none;
}

.nav {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 70px;
  padding-right: 450px;

}

.nav-icon {
  width: 40px;
}

.logo {
  width: 150px;
}





/* media query for phone res */

@media screen and (max-width:1000px) {

  

  

  #app h1 {
    font-family: "Montserrat", sans-serif;
    font-weight: 700;



    font-size: var(--font-size-h1-mobile);
    color: var(--text-primary-color);
  }

  ;

  #app h2 {
    font-family: "Montserrat", sans-serif;
    font-weight: 600;
    font-size: var(--font-size-h2-mobile);
    color: var(--text-primary-color);
  }

  ;

  .button {

    font-family: var(--font-montserrat-bold);
    font-weight: var(--font-weight-bold);
    font-size: var(--font-size-buttons-and-tabs-mobile);
    color: white;
    padding: 0.5em 3em 0.5em 3em;
    border: none;
    transition: background-color 0.3s ease;
    border-radius: 5px;
    background-color: var(--element-primary-color);
  }



  .app-container {
    display: flex;
    flex-direction: column-reverse;
  }

  .nav2 {
    position: sticky;
  bottom: 0;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  height: 40px;
  background-color: var(--element-background-color1);
  }




}

@media screen and (min-width:359px) and (max-width:400px) {
    .nav-icon {
        width:20px;
    }

    
}
</style>
