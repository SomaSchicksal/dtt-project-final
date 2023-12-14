<template> 

  <div>
    
    <DeletionConfirmPopup v-if="showPopup" @confirmed="deleteHouse" @canceled="cancelDeletion"></DeletionConfirmPopup>
    
    <House :house="house" :imageUrl="imageUrl" :goToEditPage="goToEditPage" :showDeletionConfirm="showDeletionConfirm"></House>
    
  </div>

  <!-- This component renders the info regarding the user's create house. Here the user
can then choose to either delete or edit his/her house -->
</template>

<script>
import DeletionConfirmPopup from './DeletionConfirmPopup.vue';
import House from './House.vue';
import Edit from "../assets/ic_edit@3x.png"
import Delete from "../assets/ic_delete@3x.png"

export default {
  data() {
    return {
      house: {},
      imageUrl: null,
      showPopup: false,
      edit:Edit,
      delete:Delete,
    };
  },
  mounted() {
    
    const houseIndex = this.$route.params.index;
    console.log('House index:', houseIndex);
    const selectedHouse = this.$store.state.myListings[houseIndex];
    if (selectedHouse) {
      this.house = selectedHouse;
      this.loadImage();
    }
    else {
      
      console.error(`House at index ${houseIndex} not found.`);
    }
  },

  /* as soon as the components is mounted im getting the house's infos in order to render them*/
  methods: {
    
    loadImage() {
      const reader = new FileReader();
      reader.onload = () => {
        this.imageUrl = reader.result;
      };
      
      reader.readAsDataURL(this.house.image);
    },

    //this function handles the reading of the house's picture

    
    goToEditPage() {
      const index = this.$store.state.myListings.length - 1;
      
      this.$router.push({ name: "edit-house", params: { index } });
    },
    deleteHouse() {
      const index = this.$store.state.myListings.length - 1;
      this.$store.dispatch("deleteHouse", index);
      this.showPopup = false;
      this.$router.push({ name: "home"});
    },

    //these functions handle the editing and deletion functionalities, respectively
    showDeletionConfirm() {
      
      this.showPopup = true;

      // Sets showPopup to true to display the popup
    },
    cancelDeletion() {
      this.showPopup=false;
    }
  },
  components: { DeletionConfirmPopup, House }
}

</script>