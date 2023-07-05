<template>
    <div class="component-container">
      <h3 style="text-align: center;">Picture of Rockets</h3>
      <select v-model="selectedRocket" @change="fetchRocketPhoto">
        <option value="">Select a rocket</option>
        <option v-for="rocket in rockets" :value="rocket.id" :key="rocket.id">
          {{ rocket.name }}
        </option>
      </select>
  
      <div v-if="selectedRocket && rocketPhoto" >
        <img :src="rocketPhoto" :alt="selectedRocketName"  class="rocket-image-container"/>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        rockets: [],
        selectedRocket: '',
        rocketPhoto: '',
        selectedRocketName: '',
      };
    },
    mounted() {
      this.fetchRockets()
        .then(() => {
          if (this.rockets.length > 0) {
            this.selectedRocket = this.rockets[0].id;
            this.fetchRocketPhoto();
          }
        });
    },
    methods: {
      async fetchRockets() {
        try {
          const response = await fetch('https://api.spacexdata.com/v4/rockets');
          const data = await response.json();
          this.rockets = data;
        } catch (error) {
          console.error(error);
        }
      },
      async fetchRocketPhoto() {
        if (!this.selectedRocket) {
          return;
        }
  
        try {
          const response = await fetch(`https://api.spacexdata.com/v4/rockets/${this.selectedRocket}`);
          const data = await response.json();
          this.rocketPhoto = data.flickr_images[0];
          this.selectedRocketName = data.name;
        } catch (error) {
          console.error(error);
        }
      },
    },
  };
  </script>
  
  <style>
  .rocket-image-container {
    max-height: 270px;
  }
  
  .component-container {
    background-color: #f2f2f2;
    border: 1px solid #ccc;
    border-radius: 10px;
    padding: 20px;
    margin-right: 10px;
  }

  select {
    border-radius: 8px;
    background-color: #ccc;
    color: #000000;
  }
  </style>
  