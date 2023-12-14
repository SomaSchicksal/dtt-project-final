<template>
    
    <div class="confirm-delete-popup">
        <div class="popup-content">
            <h1>Delete listing</h1>
            <p>Are you sure you want to delete this listing?</p>
            <p>This cannot be undone</p>
            <div style="display:flex; flex-direction:column; gap:10px;">
                <button @click="confirmDeletion" class="button">YES, DELETE</button>
                <button @click="cancelDeletion" class="delete-button">GO BACK</button>
            </div>
        </div>
    </div>

    <!-- this component will be rendered if the user presses the deleted button,
    to confirm the deletion of the house or not. It's basically two buttons 
   with functions attached to them that will either delete the house or hide the component -->
</template>

<script>
export default {
    data() {
        return {
            house: [],
        }
    },

    methods: {
        confirmDeletion() {
            
            this.$emit('confirmed');
        },

        // Emit an event or call a function to confirm the deletion
        cancelDeletion() {
            
            this.$emit('canceled');
        },
        // Emit an event or call a function to cancel the deletion
    },
}
</script>

<style scoped>

/* ==========================================================================
   Deletetion confirmation Component
   ========================================================================== */

/* Sub-section: Popup
   ========================================================================== */

/* popup Style
   ========================================================================== */
.confirm-delete-popup {
    display: flex;
    align-items: center;
    justify-content: center;
    position: fixed;
    top: 0;
    left: 0;
    
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);


}

/* popup content Style
   ========================================================================== */

.popup-content {
    padding:20px 80px 20px 80px;
    background-color: white;
    
    border-radius: 8px;
    max-width: 600px;
    text-align: center;
}

.popup-content p {
    font-family: "Open Sans";
    color: var(--text-secondary-color)
}

/* delete button Style
   ========================================================================== */

.delete-button {
    font-family: var(--font-montserrat-bold);
    font-weight: var(--font-weight-bold);
    font-size: var(--font-size-buttons-and-tabs-desktop);
    color: white;
    padding: 0.5em 3em 0.5em 3em;
    border: none;
    transition: background-color 0.3s ease;
    border-radius: 5px;
    background-color: var(--element-secondary-color);
}

/* Sub-section: Media Queries
   ========================================================================== */

@media screen and (min-width:359px) and (max-width:600px) {
    .confirm-delete-popup {
    
    z-index:1000;


}

.popup-content {
    padding:20px 20px 20px 20px;
    background-color: white;
    
    border-radius: 8px;
    max-width: 300px;
    text-align: center;
}
}
</style>