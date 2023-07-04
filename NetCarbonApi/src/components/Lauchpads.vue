<template>
  <div class="component-container">
    <h3 style="text-align: center;">Liste des Launchpads SpaceX</h3>
    <select v-model="selectedLaunchpad" @change="fetchRockets">
      <option value="">Sélectionnez un Launchpad</option>
      <option v-for="launchpad in launchpads" :key="launchpad.id" :value="launchpad">{{ launchpad.name }}</option>
    </select>
    <div v-if="selectedLaunchpad">
      <h2>Launchpad sélectionné :</h2>
      <p>{{ selectedLaunchpad.full_name }}</p>
      
      <h2>Fusées envoyées depuis le Launchpad :</h2>
      <!-- <p v-for="LaunchRocket in selectedLaunchpad.rockets " :key="LaunchRocket" :value="LaunchRocket">{{ LaunchRocket }}</p> -->
      <div v-for="LaunchRocket in selectedLaunchpad.rockets " :key="LaunchRocket" :value="LaunchRocket">
        <div v-for="rocket in rockets" :key="rocket.id">
          <p v-if="LaunchRocket == rocket.id">{{ rocket.name }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      launchpads: [],
      selectedLaunchpad: null,
      rockets: [], 
      RocketsLauchpad : [],
    };
  },
  mounted() {
    this.fetchLaunchpads();
  },
  methods: {
    fetchLaunchpads() {
      fetch('https://api.spacexdata.com/v4/launchpads')
        .then(response => response.json())
        .then(data => {
          this.launchpads = data;
        })
        .catch(error => {
          console.error('Erreur lors de la récupération des launchpads', error);
        });
    },
    fetchRockets() {
      if (this.selectedLaunchpad) {
        fetch(`https://api.spacexdata.com/v4/rockets?launchpad=${this.selectedLaunchpad.id}`)
          .then(response => response.json())
          .then(data => {
            this.rockets = data;
          })
          .catch(error => {
            console.error('Erreur lors de la récupération des fusées', error);
          });
      } else {
        this.rockets = [];
      }
    }
  }
};
</script>

<style>
.component-container {
  background-color: #f2f2f2;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  margin-right: 10px;
}
</style>