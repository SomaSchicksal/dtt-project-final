<template>
    <div class="create-form" :style="'background-image: url(' + background + ')'">
        <BackToOverview></BackToOverview>
        <h1>Create new listing</h1>
        <Form :house="newHouse" :handleImageUpload="handleImageUpload" :msg="msg" :isFormValid="isFormValid"
            :validateNumberField="validateNumberField" :validateTextField="validateTextField" :submitForm="submitForm"
            :isEditMode="isEditMode" :houseImage="url" @deleteUploadedImage="deleteUploadedImage"></Form>
    </div>

    <!-- This component allows the user to create a house by filling some fields,
        submit the form and then be redirected to a page where its details are shown
      -->
</template>

<script>

import Form from "./Form.vue"
import Back from "@/assets/ic_back_grey@3x.png"
import Background from "../assets/img_background@3x.png"
import BackToOverview from "@/components/BackToOverview.vue";


export default {
    data() {
        return {
            houses: [],
            houseImage: null,
            houseImage2: null,
            url: null,
            isHouseNumberValid: true,
            msg: [],
            isEditMode: false,
            back: Back,
            background: Background,
            newHouse: {
                street: "",
                number: null,
                addition: "",
                postalCode: null,
                city: "",
                image: null,
                price: null,
                size: null,
                garage: "",
                bedrooms: null,
                bathrooms: null,
                constructionDate: null,
                details: "",
                madeByMe: true
            }
        }
    },



    computed: {
        isFormValid() {


            return this.newHouse.details.trim() !== ""
                && this.houseImage !== null
                && this.newHouse.street.trim() !== ""

                && this.newHouse.city.trim() !== ""
                && this.newHouse.garage.trim() !== ""
                && (this.newHouse.constructionDate !== null && !isNaN(this.newHouse.constructionDate))
                && (this.newHouse.price !== null && !isNaN(this.newHouse.price))
                && (this.newHouse.number !== null && !isNaN(this.newHouse.number))
                && this.newHouse.postalCode.trim() !== ""
                && (this.newHouse.size !== null && !isNaN(this.newHouse.size))
                && (this.newHouse.bedrooms !== null && !isNaN(this.newHouse.bedrooms))
                && (this.newHouse.bathrooms !== null && !isNaN(this.newHouse.bathrooms));







        },

        // Check if all required fields are completed
    },



    methods: {
        submitForm() {




            if (this.isFormValid) {

                /*if the form is valid the new house will be created, the newHouse object and
            houseImage will be then passed to the house component rendered in the myHouse 
            component, as props */
                this.$store.dispatch('addNewHouse', { newHouse: this.newHouse, image: this.houseImage });

                const index = this.$store.state.myListings.length - 1;



                console.log('Index value:', index);
                this.$router.push({ name: 'myhouse-details', params: { index } });

                // Redirect to the details page with the index as a parameter


            }

        },



        handleImageUpload(event) {
            const file = event.target.files[0];


            if (file) {

                this.houseImage = file;
                this.houseImage2 = file;
                this.readImage(file);


            }
        },

        readImage(file) {
            const reader = new FileReader();

            reader.onload = () => {

                this.url = reader.result;




            };


            reader.readAsDataURL(file);

        },

        //function that handles the uploading of the image from the user's device



        navigateToHome() {
            this.$router.push({ name: 'home' });
        },




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
        //some validation logic

        deleteUploadedImage() {
            this.imageUrl = null;
        }




    },

    components: { Form, BackToOverview }
}
</script>

<style >
.create-form {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding-left: 50px;
    background-image: url('@/assets/img_background@3x.png');
    background-size: cover;

}

/* Sub-section: Media Queries
   ========================================================================== */

@media screen and (min-width:320px) and (max-width:400px) {

    .create-form {
        padding-left: 10px;


    }

}

@media screen and (min-width:767px) and (max-width:900px) {


    .create-form {
        display:flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding-left: 0;


    }






}
</style>