<template>
    <div>
        <div class="back">
            <img :src="back" @click="navigateToHome">
            <h2 class="back-h2">Back to overview</h2>
        </div>
    </div>

    <!-- Basic form component that will be rendered every time you see 
    the back to overview element, it basically redirect the user to the homepage if it clicks it -->
</template>

<script>
import Back from "@/assets/ic_back_grey@3x.png"

export default {
    data() {
        return {
            back: Back,
        }
    },

    methods: {
        navigateToHome() {
            this.$router.push({ name: 'home' });
        },
    }
}
</script>

<style>
/* ==========================================================================
   Backtooverview Component
   ========================================================================== */

/* Sub-section: Back
   ========================================================================== */

/* back element Style
   ========================================================================== */
.back {
    display: flex;
    align-items: center;
    gap: 15px;
    padding-top: 40px;
    padding-bottom: 10px;
}

.back img {

    width: 25px;
    height: 25px;
    cursor: pointer;
}

/* Sub-section: Media Queries
   ========================================================================== */

@media screen and (min-width:320px) and (max-width:600px) {

    .back {
        display: flex;
        align-items: center;
        gap: 15px;
        padding-top: 40px;
        padding-bottom: 10px;
        padding:0;
    }

    .back-h2 {
        display:none;
    }

    .back img {

        width: 15px;
        height: 15px;
        cursor: pointer;
    }
}
</style>