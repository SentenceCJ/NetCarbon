<template>
  <div class="component-container">
    <h3 style="text-align: center;">Liste des Launchpads SpaceX</h3>
    <select v-model="selectedLaunchpad" @change="fetchRockets">
      <option value="">Sélectionnez un Launchpad</option>
      <option v-for="launchpad in launchpads" :key="launchpad.id" :value="launchpad">{{ launchpad.name }}</option>
    </select>
    <div v-if="selectedLaunchpad">
      <br>
      <h3>Launchpad sélectionné :</h3>
      <p>Nom Complet : {{ selectedLaunchpad.full_name }}</p>
      <p>Région : {{ selectedLaunchpad.region }}</p>
      
      <p>Nombre de lancements : {{ selectedLaunchpad.launch_attempts }}</p>
      <p>Nombre de lancements réussis : {{ selectedLaunchpad.launch_successes }}</p>
      <br>
      <h3>Fusées envoyées depuis le Launchpad :</h3>
      <div v-for="rocket in RocketsLauchpad" :key="rocket.id">
        <p>{{ rocket.name }}</p>
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
      RocketsLauchpad: [],
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
          if (this.launchpads.length > 0) {
            this.selectedLaunchpad = this.launchpads[0]; // Sélectionne le premier launchpad par défaut
            this.fetchRockets(); // Charge les fusées du launchpad sélectionné par défaut
          }
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
            this.RocketsLauchpad = this.rockets.filter(rocket => this.selectedLaunchpad.rockets.includes(rocket.id));
          })
          .catch(error => {
            console.error('Erreur lors de la récupération des fusées', error);
          });
      } else {
        this.rockets = [];
        this.RocketsLauchpad = [];
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
