<template>
  
  <div class="about">
    <div class="info-container">
      <h1>About the project</h1>
      <h2>This is a house listing application made as an assessment for Dtt Company.</h2>
      <h2>Users can look for a house, create one or simply like existing ones</h2>
    </div>

    <div class="info-container">
      <h1>About Dtt</h1>
      <h2>Dtt is a full-service digital agency located in the heart of Amsterdam.</h2>
      <h2>It's renown in the marketplace for its dedication to his clients, 
        making their work process accessible and helping them maximising their results</h2>
    </div>
    <div class="info-container">
      <h1>About the developer</h1>
      <h2>I'm Joel and i am a self taught web developer with a bachelor degree in philosophy</h2>
      <h2>Passionate about creating efficient, and user-friendly interfaces, 
        I am committed on staying up to date with the latest technologies
      </h2>
    </div>
  </div>
  <!-- Basic component that renders info about the company, the project and the developer -->
</template>

<style>

/* ==========================================================================
   About Component
   ========================================================================== */

/* Sub-section: about
   ========================================================================== */

/* About section Style
   ========================================================================== */
.about {
  display:flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: var(--element-tertiary-color2);
  padding:60px;
}

.info-container {
  background-color: var(--element-background-color2);
  
  padding:10px;
}

/* Sub-section: Media Queries
   ========================================================================== */

@media screen and (min-width:320px) and (max-width:450px) {
    .about {
      display:flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap:20px;
      
    }

    .info-container {
      width:200px;
    }

    
}

@media screen and (min-width:390px) and (max-width:450px) {
    .about {
      display:flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap:20px;
      
    }

    .info-container {
      width:200px;
    }

    
}
</style>
