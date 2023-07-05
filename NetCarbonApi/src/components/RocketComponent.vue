<template>
  <div class="component-container">
    <h3 style="text-align: center;">Liste des fusées :</h3>
    <select v-model="selectedRocket">
      <option v-for="rocket in rockets" :key="rocket[0]" :value="rocket[0]">{{ rocket[1] }}</option>
    </select>
    <div v-if="selectedRocketDetails">
      <br>
      <p>Nom : {{ selectedRocketDetails.name }}</p>
      <p>ID : {{ selectedRocketDetails.id }}</p>
      <p>Taille : {{ selectedRocketDetails.height.meters }} m</p>
      <p>Diamètre : {{ selectedRocketDetails.diameter.meters }} m</p>
      <p>Poids : {{ selectedRocketDetails.mass.kg }} kg</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rockets: [],
      selectedRocket: null,
      selectedRocketDetails: null,
    };
  },
  mounted() {
    this.getRockets();
  },
  methods: {
    async getRockets() {
      try {
        const response = await fetch('https://api.spacexdata.com/v4/rockets');
        const data = await response.json();

        this.rockets = data.map(function (rocket) {
          return [rocket.id, rocket.name];
        });

        if (this.rockets.length > 0) {
          this.selectedRocket = this.rockets[0][0]; // Sélectionne la première fusée par défaut
          this.getRocketDetails(); // Charge les détails de la fusée sélectionnée par défaut
        }
      } catch (error) {
        console.error('Une erreur est survenue :', error);
      }
    },
    async getRocketDetails() {
      try {
        const response = await fetch(`https://api.spacexdata.com/v4/rockets/${this.selectedRocket}`);
        const data = await response.json();
        this.selectedRocketDetails = data;
      } catch (error) {
        console.error('Une erreur est survenue :', error);
      }
    },
  },
  watch: {
    selectedRocket() {
      this.getRocketDetails();
    },
  },
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
