<template>
    <div class="component-container3">
      <h3 style="text-align: center;">List of Landpads</h3>
      <select v-model="selectedLandpad" @change="getLandpadInfo">
        <option value="">Select a Landpad</option>
        <option v-for="landpad in landpads" :value="landpad.id" :key="landpad.id">
          {{ landpad.name }}
        </option>
      </select>
      <div v-if="landpadInfo">
        <br>
        <h3> Landpad selected :</h3>
        <br>
        <p>Full Name : {{ landpadInfo.full_name }}</p>
        <p>Region : {{ landpadInfo.region }}</p>
        <p>Locality : {{ landpadInfo.locality }}</p>
        <p>Landing Attempts : {{ landpadInfo.landing_attempts }}</p>
        <p>Successful Landing : {{ landpadInfo.landing_successes }}</p>
      </div>
    </div>
  </template>
  
  <script>
  import { ref, onMounted } from 'vue';
  
  export default {
    name: 'LandpadList',
    setup() {
      const landpads = ref([]);
      const selectedLandpad = ref('');
      const landpadInfo = ref(null);
  
      
      const fetchLandpads = async () => {
        try {
          const response = await fetch('https://api.spacexdata.com/v4/landpads');
          const data = await response.json();
          landpads.value = data;
          if (data.length > 0) {
            selectedLandpad.value = data[0].id; 
            getLandpadInfo(); 
          }
        } catch (error) {
          console.error(error);
        }
      };
  
      
      const getLandpadInfo = async () => {
        try {
          const response = await fetch(`https://api.spacexdata.com/v4/landpads/${selectedLandpad.value}`);
          const data = await response.json();
          landpadInfo.value = data;
        } catch (error) {
          console.error(error);
        }
      };
  
      onMounted(fetchLandpads);
  
      return {
        landpads,
        selectedLandpad,
        landpadInfo,
        getLandpadInfo
      };
    }
  };
  </script>
  
  <style>
  .component-container3 {
    background-color: #f2f2f2;
    border: 1px solid #ccc;
    border-radius: 10px;
    padding: 20px;
    margin-right: 10px;
    margin-left: 12px ;
    width: 300px;
  }
  </style>
  