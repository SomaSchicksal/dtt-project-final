<template>
    <div v-if="house" class="form-container">

        <p class="requirement"> Required fields are marked with an asterisk
            (<abbr class="req" title="required">*</abbr>).</p>
        <form @submit.prevent="submitForm" novalidate class="form">
            <!-- Basic form component that will be rendered every time you see 
    a form in the application -->
            <label for="streetName" class="field-name">Street name<span class="req" title="required">*</span></label>
            <input v-model="house.street" type="text" id="streetName" required
                @input="validateTextField('street', house.street)" placeholder="Enter the street name">
            <p v-if="msg['street']" class="error-message">Error: {{ msg['street'] }}</p>

            <!-- each field is binded to a property of the house object
            , when i type on the field the property is updated and 
            a function is fired that checks for validation errors. 
            If a validation error is present a error message will be conditionally rendered
         -->

            <div style="display:flex; gap:10px">
                <div style="display:flex; flex-direction:column">
                    <label for="houseNumber" class="field-name">House number<span class="req"
                            title="required">*</span></label>
                    <input v-model="house.number" type="text" id="houseNumber" required
                        @input="validateNumberField('number', house.number)" placeholder="Enter house number">
                    <p v-if="msg['number']" class="error-message">Error: {{ msg['number'] }}</p>
                </div>


                <div style="display:flex; flex-direction:column">
                    <label for="houseAddition" class="field-name">Addition (optional)</label>
                    <input v-model="house.addition" type="text" id="houseAddition"
                        @input="validateTextField('addition', house.addition)" placeholder="e.g.A">
                    <p v-if="msg['addition']" class="error-message">Error: {{ msg['addition'] }}</p>
                </div>
            </div>

            <label for="housePostalCode" class="field-name">Postal code<span class="req" title="required">*</span></label>
            <input v-model="house.postalCode" type="text" id="housePostalCode" required placeholder="e.g. 1000 AA">
            <p v-if="msg['postalCode']" class="error-message">Error: {{ msg['postalCode'] }}</p>

            <label for="houseCity" class="field-name">City<span class="req" title="required">*</span></label>
            <input v-model="house.city" type="text" id="houseCity" required @input="validateTextField('city', house.city)"
                placeholder="e.g. Utrecht">
            <p v-if="msg['city']" class="error-message">Error: {{ msg['city'] }}</p>

            <label for="houseImage" style="display:flex; flex-direction: column;">
                <div style="display:flex; align-items: center; justify-content: center;" class="field-name">
                    Upload image
                    <span class="req" title="required">*</span>
                </div>
                <div style="display:flex;flex-direction: column; position:relative">
                    <img v-if="houseImage" :src="houseImage" alt="uploaded-image" class="uploaded-image">
                    <img v-else-if="imageUrl" :src="imageUrl" alt="uploaded-image" class="uploaded-image">
                    <img v-else :src="upload" alt="default-image" class="uploaded-image">
                    <img v-if="houseImage || imageUrl" :src="clear" alt="clear-icon" class="clear-icon"
                        @click="deleteUploadedImage">
                </div>

            </label>
            <input type="file" id="houseImage" @change="handleImageUpload" required style="display:none">



            <label for="housePrice" class="field-name">Price<span class="req" title="required">*</span></label>
            <input v-model="house.price" type="text" id="housePrice" required
                @input="validateNumberField('price', house.price)" placeholder="e.g. $150.000">
            <p v-if="msg['price']" class="error-message">Error: {{ msg['price'] }}</p>

            <div style="display:flex; gap:10px">
                <div style="display:flex; flex-direction:column">
                    <label for="houseSize" class="field-name">Size<span class="req" title="required">*</span></label>
                    <input v-model="house.size" type="text" id="houseSize" required
                        @input="validateNumberField('size', house.size)" placeholder="e.g 60m2">
                    <p v-if="msg['size']" class="error-message">Error: {{ msg['size'] }}</p>
                </div>

                <div style="display:flex; flex-direction:column; margin-left:auto">
                    <label for="houseGarage" class="field-name">Garage<span class="req" title="required">*</span></label>
                    <select v-model="house.garage" id="houseGarage" required>
                        <option value="yes">Yes</option>
                        <option value="no">No</option>
                    </select>
                </div>
            </div>

            <div style="display:flex; gap:10px">
                <div style="display:flex; flex-direction:column">
                    <label for="houseBedrooms" class="field-name">Bedrooms<span class="req"
                            title="required">*</span></label>
                    <input v-model="house.bedrooms" type="text" id="houseBedrooms" required
                        @input="validateNumberField('bedrooms', house.bedrooms)" placeholder="Enter amount">
                    <p v-if="msg['bedrooms']" class="error-message">Error: {{ msg['bedrooms'] }}</p>
                </div>

                <div style="display:flex; flex-direction:column">
                    <label for="houseBathrooms" class="field-name">Bathrooms<span class="req"
                            title="required">*</span></label>
                    <input v-model="house.bathrooms" type="text" id="houseBathrooms" required
                        @input="validateNumberField('bathrooms', house.bathrooms)" placeholder="Enter amount">
                    <p v-if="msg['bathrooms']" class="error-message">Error: {{ msg['bathrooms'] }}</p>
                </div>
            </div>

            <label for="houseConstructionDate" class="field-name">Construction date<span class="req"
                    title="required">*</span></label>
            <input v-model="house.constructionDate" type="text" id="houseConstructionDate" required placeholder="e.g. 1990"
                @input="validateNumberField('date', house.constructionDate)">
            <p v-if="msg['date']" class="error-message">Error: {{ msg['date'] }}</p>

            <label for="houseDetails" class="field-name">Description<span class="req" title="required">*</span></label>
            <textarea v-model="house.details" type="text" id="houseDetails" required rows="8" cols="50"
                placeholder="Enter description"></textarea>
            <p v-if="msg['details']" class="error-message">Error: {{ msg['details'] }}</p>




            <div v-if="!isFormValid" class="error-message">
                <p>{{ isEditMode ? "" : "Please complete all required fields. " }}</p>
            </div>
            <!-- if all the required fields arent completed an error message
            will be conditionally rendered -->



            <button v-if="isEditMode" type="submit" style="margin-left:auto" class="button">Save</button>
            <button v-else :disabled="!isFormValid" type="submit"
                :class="{ 'valid-form': isFormValid, 'invalid-form': !isFormValid }">Post</button>

            <!-- a function will be fired if u press the post button that 
            creates a new house -->
        </form>
    </div>
    <div v-else>
        <h1>Loading...</h1>
    </div>
</template>

<script>

import Upload from "../assets/ic_upload@3x.png"
import ClearWhite from "../assets/ic_clear_white@3x.png"


export default {

    data() {
        return {
            hello: "",
            upload: Upload,
            clear: ClearWhite


        }
    },

    props: {
        house: {
            type: Object,
            required: true
        },

        imageUrl: {
            type: String
        },

        houseImage: {
            type: String
        },

        isEditMode: {
            type: Boolean,
            default: false,
        },

        handleImageUpload: {

        },

        msg: {
            type: Array
        },

        isFormValid: {
            type: Boolean,
            default: false,

        },

        validateNumberField: {

        },

        validateTextField: {

        },

        submitForm: {

        }
    },

    created() {
        console.log('Received props in House component:', this.house);
        console.log("received image", this.imageUrl)
    },

    methods: {
        deleteUploadedImage(event) {
            if (event.target.classList.contains('clear-icon')) {

                this.$emit('deleteUploadedImage');
            }



        }
    }




}
</script>

<style>
/* ==========================================================================
   Form Component
   ========================================================================== */

/* Sub-section: Form
   ========================================================================== */

/* Initial Requirement Style
   ========================================================================== */
.requirement {
    font-family: "0pen Sans";
    color: var(--text-secondary-color)
}

.req {
    font-size: 30px;
}

/* Form Style
   ========================================================================== */
.form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

/* Label Style
   ========================================================================== */

.form label {
    font-family: "Montserrat", sans-serif;
    font-weight: 600;
    font-size: var(--font-size-input-field-title-desktop);
}

/* Input Style
   ========================================================================== */

.form input {

    font-family: "Open Sans", sans-serif;
    font-weight: 400;
    font-size: var(--font-size-input-field-desktop);
    padding: 20px;
    border: none;
    border-radius: 5px;
}

.form select {
    padding: 10px;
}

/* Errors Style
   ========================================================================== */

.error-message {
    font-family: "Montserrat", sans-serif;
    font-weight: 500;
    font-size: var(--font-size-error-message-desktop);
    color: red;
}

/* Validation Style
   ========================================================================== */

.valid-form {

    font-family: "Montserrat", sans-serif;
    font-weight: 700;
    font-size: var(--font-size-buttons-and-tabs-desktop);
    color: white;
    border-radius: 5px;
    background-color: var(--element-primary-color);
    padding: 0.5em 3em 0.5em 3em;
    border: none;
    margin-left: auto;
}

.invalid-form {
    border-radius: 5px;
    background-color: rgba(235, 84, 64, 0.7);
    font-family: "Montserrat", sans-serif;
    font-weight: 700;
    font-size: var(--font-size-buttons-and-tabs-desktop);
    color: white;
    padding: 0.5em 3em 0.5em 3em;
    border: none;
    margin-left: auto;
}

/* Uploaded image Style
   ========================================================================== */

.uploaded-image {
    width: 100px;
}

/* Clear icon Style
   ========================================================================== */

.clear-icon {
    width: 30px;
    position: absolute;
    left: 80px;
    top: 0px;
}

/* Sub-section: Media Queries
   ========================================================================== */


@media screen and (min-width:320px) and (max-width:900px) {

    

    .form {
        display: flex;
        flex-direction: column;
        
        width:90%;
        
        
    }


    .req {
        font-size: 15px;
    }

    .form label {
        font-family: "Montserrat", sans-serif;
        font-weight: 600;
        font-size: var(--font-size-input-field-title-mobile);
        text-align: left;
    }

    .form input {

        font-family: "Open Sans", sans-serif;
        font-weight: 400;
        font-size: var(--font-size-input-field-mobile);
        
        border: none;
        border-radius: 5px;
        
    }

    .error-message {
        font-family: "Montserrat", sans-serif;
        font-weight: 500;
        font-size: var(--font-size-error-message-mobile);
        color: red;
    }

    .valid-form {

        font-family: "Montserrat", sans-serif;
        font-weight: 700;
        font-size: var(--font-size-buttons-and-tabs-mobile);
        color: white;
        border-radius: 5px;
        background-color: var(--element-primary-color);
        margin:0;
        border: none;

    }

    .invalid-form {
        border-radius: 5px;
        background-color: rgba(235, 84, 64, 0.7);
        font-family: "Montserrat", sans-serif;
        font-weight: 700;
        font-size: var(--font-size-buttons-and-tabs-mobile);
        color: white;
        margin:0;
        
        border: none;

    }

    







}


</style>