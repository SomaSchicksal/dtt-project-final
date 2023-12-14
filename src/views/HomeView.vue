<template>
  
  <div class="home">
    <div class="first-block">
      <div class="title-container">
        <h1>Houses</h1>
      </div>
      <div class="button-container">
        <router-link to="/houseForm" v-if="isLargeScreen"><button class="button">+ CREATE NEW</button></router-link>
        <router-link to="/houseForm" v-else><img :src="plus" class="plus-icon"></router-link>
      </div>
    </div>

    <div class="second-block">
      
      <div class="searchbar-container">
        <input v-model="search" placeholder="Search for a house" @input="filterHouses" class="searchbar" />
        <img :src="clear" v-if="search !== ''" @click="clearSearchInput" class="clear-button">
      </div>

      <!-- typing in the searchbar will fire the function that handle the filtering of the houses -->
      <div>
        
        <button @click="sortByPrice" class="button button1">Price</button>
        <button @click="sortBySize" class="button button2">Size</button>
        <!-- These buttons fire the function that handle the filtering by price and size -->
      </div>
    </div>
    
    <div class="result-indication" v-if="filteringOccurred">
      <h2 v-if="filteredHouses.length > 0">
        {{ filteredHouses.length }} result{{ filteredHouses.length !== 1 ? 's' : '' }} found.
      </h2>
      <div style="display:flex; flex-direction: column; align-items: center; padding: 50px; height:800px;" v-else>
        <img :src="empty" style="width:300px;">
        <div style="font-family: 'Montserrat';">
          <p>No results found.</p>
          <p>Please try another keyword.</p>
        </div>
      </div>
      <!-- a div will indicate the results of the search if the user uses the searchbar -->

      <!-- This component renders the homepage, as soon as it's loaded a list of houses will
  gotten from the api will be rendered. The user can filter them by price or size, or look
for a specific house by typing in the searchbar. Here the user can even create a house-->
    </div>

    
    <div class="third-block" v-for="house in houses" :key="house.id">

      <House :house="house"></House>



    </div>
    <!-- simple for loop to render all houses i get from the api call, for each house i get 
  i render a house component that will render its details -->
  </div>
</template>

<script>


import House from '@/components/House.vue';
import Empty from "../assets/img_empty_houses@3x.png"
import Clear from "../assets/ic_clear@3x.png"
import Plus from "../assets/ic_plus_grey@3x.png"


export default {
  data() {
    return {
      houses: [],
      search: "",
      filteredHouses: [],
      filteringOccurred: false,
      empty: Empty,
      clear: Clear,
      plus:Plus,
      isLargeScreen: true


    }
  },
  
  methods: {


    handleWindowSize() {
      // Update isLargeScreen based on window size
      this.isLargeScreen = window.innerWidth > 700; 
    },
    
    getHouses() {
      this.houses = [];
      this.$store.dispatch("fetchApiData").then(() => {
        this.houses = this.$store.state.apiData;


      })
    },
    //function that makes the api call



    

    sortByPrice() {
      this.houses.sort((a, b) => a.price - b.price);

    },

    sortBySize() {
      this.houses.sort((a, b) => a.size - b.size);

    },
    //filtering functions

    



    filterHouses() {

      const searchTerm = this.search.toLowerCase();
      this.houses = this.$store.state.apiData.filter((house) => {
        return house.location.city.toLowerCase().includes(searchTerm)
          || house.location.zip.toLowerCase().includes(searchTerm)
          || house.size.toString().includes(searchTerm)
          || house.price.toString().includes(searchTerm);
      });
      this.filteredHouses = this.houses;
      this.filteringOccurred = true;


    },

    clearSearchInput() {
      this.search = '';
      this.getHouses()

    }
  },
  //logic that handles the filtering of the search bar

  computed: {

    
    filteredHouses() {
      const searchTerm = this.search.toLowerCase();
      return this.houses.filter((house) => {
        const cityMatch = house.location.city.toLowerCase().includes(searchTerm);
        const zipMatch = house.location.zip.toLowerCase().includes(searchTerm);


        const sizeMatch = house.size.toString().includes(searchTerm);
        const priceMatch = house.price.toString().includes(searchTerm);

        return cityMatch || zipMatch || sizeMatch || priceMatch;
      })
    }
  },
  //function that handles the filtering of the searchbar



  mounted() {

    
    this.getHouses();
    console.log(this.houses);

    this.handleWindowSize();
    // Sets initial screen size

    
    window.addEventListener('resize', this.handleWindowSize);
    // Adds event listener for window resize


  },

  // i make the api call as soon as the component is mounted with this function

  beforeDestroy() {
    
    window.removeEventListener('resize', this.handleWindowSize);
    // Remove event listener to prevent memory leaks
  },

  components: { House }

}
</script>

<style scoped>

/* ==========================================================================
   Homeview Component
   ========================================================================== */

/* Sub-section: First Block
   ========================================================================== */

/* Home Style
   ========================================================================== */
.home {
  display: flex;
  flex-direction: column;
  background-color: var(--element-background-color1);
  padding-left: 200px;
  padding-right: 200px;
  gap: 20px;
}

.first-block {
  display: flex;
  justify-content: space-between;
  align-items: center;

}

.first-block button {
  padding: 0.5em 1em 0.5em 1em !important;

}

.plus-icon {
  width:20px;
}
/* Sub-section: Second Block
   ========================================================================== */
.second-block {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.searchbar-container {
  position:relative;
}

.searchbar {
  padding: 1em 12em 1em 0;
  background-color: var(--element-tertiary-color1);
  border-radius: 0.5em;
  border: none;
  
}

.clear-button {
  cursor: pointer;
  width: 25px;
  position: absolute;
  right: 5px;
  top: 50%; 
  transform: translateY(-50%);
  
}

.second-block .searchbar::placeholder {
  font-family: "Open Sans", sans-serif;
  font-weight: 400;
  color: var(--element-tertiary-color2);
  font-size: var(--font-size-input-field-desktop);

}

.second-block .button1 {
  border-top-right-radius: 0 !important;
  border-bottom-right-radius: 0 !important;
}

.second-block .button2 {
  border-top-left-radius: 0 !important;
  border-bottom-left-radius: 0 !important;
  background-color: var(--element-tertiary-color2) !important;
}

.button {
  display: inline-block;
}

.active-button {
  background-color: var(--element-tertiary-color2);
  color: white;
}

.active-button2 {
  background-color: var(--element-background-color1);
  color: white;
}


@media screen and (max-width:400px) {
  .home {
  display: flex;
  flex-direction: column;
  padding:0;
  background-color: var(--element-background-color1);
  gap: 20px;
  
}

.first-block {
  display:flex;
  justify-content: center;
  align-items: center;
  
  
}

.title-container {
    /* Center h1 within the available space */
    margin-right: auto;
    margin-left: auto; /* Add this line */
  }


.button-container {
  margin-left:auto;
}

.second-block {
  display: flex;
  flex-direction:column;
  gap:20px;
}

.searchbar {
  padding: 1em 12em 1em 0;
  background-color: var(--element-tertiary-color1);
  border-radius: 0.5em;
  border: none;
  
}

.second-block .searchbar::placeholder {
  font-family: "Open Sans", sans-serif;
  font-weight: 400;
  color: var(--element-tertiary-color2);
  font-size: var(--font-size-input-field-mobile);

}

.second-block .button1 {
  border-top-right-radius: 0 !important;
  border-bottom-right-radius: 0 !important;
  padding-left:60px;
  padding-right: 60px;
}

.second-block .button2 {
  border-top-left-radius: 0 !important;
  border-bottom-left-radius: 0 !important;
  background-color: var(--element-tertiary-color2) !important;
  padding-left:60px;
  padding-right: 60px;
}


}


@media screen and (min-width: 401px) and (max-width:600px) {
  .home {
  display: flex;
  flex-direction: column;
  padding:0;
  background-color: var(--element-background-color1);
  gap: 20px;
  width:100%;
  
}

.first-block {
  display:flex;
  justify-content: center;
  align-items: center;
  
  
}

.title-container {
  flex-grow: 1;
}


.button-container {
  margin-left: auto;
}

.second-block {
  display: flex;
  flex-direction:column;
  gap:20px;
}

}




@media screen and (min-width: 765px) and (max-width:950px) {
  .home {
  display: flex;
  flex-direction: column;
  padding:0;
  background-color: var(--element-background-color1);
  gap: 20px;
  width:100%;
  
}

.first-block {
  display:flex;
  justify-content: center;
  align-items: center;
  
  
}

.title-container {
  flex-grow: 1;
}


.button-container {
  margin-left: auto;
}

.second-block {
  display: flex;
  flex-direction:column;
  gap:20px;
}

}

@media screen and (min-width: 951px) and (max-width:1025px) {
  .home {
  display: flex;
  flex-direction: column;
  padding:0;
  background-color: var(--element-background-color1);
  gap: 20px;
  width:100%;
  
}

.first-block {
  display:flex;
  justify-content: center;
  align-items: center;
  
  
}

.title-container {
  flex-grow: 1;
}


.button-container {
  margin-left: auto;
}

.second-block {
  display: flex;
  flex-direction:column;
  gap:20px;
}

}

</style>