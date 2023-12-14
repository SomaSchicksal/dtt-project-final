<template>
    <div class="create-form" :style="'background-image: url(' + background + ')'">
        <!-- Form to edit all the info of your house, loaded with the original info
          as default values --> 
        <BackToOverview></BackToOverview>
        <h1>Edit Listing</h1>

        <div>
            <Form :house="house" :handleImageUpload="handleImageUpload" :msg="msg"
                :validateNumberField="validateNumberField" :validateTextField="validateTextField" :submitForm="submitForm"
                :isEditMode="isEditMode" :imageUrl="imageUrl"></Form>
        </div>
    </div>
</template>
  
<script>

import Form from "./Form.vue"
import BackToOverview from "@/components/BackToOverview.vue";
import Back from "@/assets/ic_back_grey@3x.png"
import Background from "../assets/img_background@3x.png"

export default {
    data() {
        return {
            house: {},
            imageUrl: null,
            houseImage: null,
            selectedImage: null,
            msg: [],
            back:Back,
            background:Background,
            isEditMode: true
        }
    },



    mounted() { 
        const houseIndex = this.$route.params.index;
        const selectedHouse = this.$store.state.myListings[houseIndex];
        /*as soon as the component is mounted i want to get the info
        of the specific house im working with, the one i want to edit */

        

        if (selectedHouse) {
            this.house = { ...selectedHouse };
            this.loadImage();
            console.log(this.house)
        } else {
            
            console.error(`House at index ${houseIndex} not found.`);
        }
        /* i then render the house and his image*/
    },

    methods: {
        loadImage() {
            const reader = new FileReader();
            reader.onload = () => {
                this.imageUrl = reader.result;

            };


            reader.readAsDataURL(this.house.image);
            console.log(this.house.image)
            console.log(this.imageUrl)
        },

        

        handleImageUpload(event) {
            const file = event.target.files[0];

            if (file) {

                this.selectedImage = file;


                this.readImage(file);


            }
        },

        readImage(file) {
            const reader = new FileReader();

            reader.onload = () => {

                this.imageUrl = reader.result;




            };


            reader.readAsDataURL(file);

        },

        /* this method is used to load the image in case the user chooses
        to update a new image*/
        

        validateNumberField(fieldName, value) {
            if (/^[0-9]+$/.test(value)) {
                this.msg[fieldName] = '';
            } else {
                this.msg[fieldName] = 'Invalid input. Please enter a number.';
            }
        },

        validateTextField(fieldName, value) {
            if (/^[a-zA-Z]+$/.test(value)) {
                this.msg[fieldName] = '';
            } else {
                this.msg[fieldName] = 'Invalid input. Please enter a letter.';
            }
        },

        // some validation logic follows

        

        submitForm() {
            const houseIndex = this.$route.params.index;
            const imageData = this.imageUrl ? { image: this.imageUrl } : {};
            if (this.selectedImage) {
                this.house.image = this.selectedImage;
            }


            this.$store.dispatch('saveHouseChanges', {
                index: houseIndex,
                house: this.house,
                ...imageData,
            });








            
            this.$router.push({ name: 'myhouse-details', params: { index: houseIndex } });

            // Redirect back to the house details page
        },

        /* the function that im going to fire when i submit the form:
        i take the house object, its index and the image as separate parameters
        the dispatch an action to save the changes made to the house and 
        redirect the user to the house's detail page*/

        navigateToHome() {
            this.$router.push({ name: 'home' });
        },
    },

    components: { Form, BackToOverview }
}

</script>