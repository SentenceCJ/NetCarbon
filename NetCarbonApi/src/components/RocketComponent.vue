<template>
  <div class="component-container1">
    <h3 style="text-align: center;">List of Rockets :</h3>
    <select v-model="selectedRocket">
      <option v-for="rocket in rockets" :key="rocket[0]" :value="rocket[0]">{{ rocket[1] }}</option>
    </select>
    <div v-if="selectedRocketDetails">
      <br>
      <h3>Details :</h3>
      <p>Name : {{ selectedRocketDetails.name }}</p>
      <p>First Flight Date : {{ selectedRocketDetails.first_flight }}</p>
      <p>Height : {{ selectedRocketDetails.height.meters }} m</p>
      <p>Diameter : {{ selectedRocketDetails.diameter.meters }} m</p>
      <p>Mass : {{ selectedRocketDetails.mass.kg }} kg</p>
      <p>Propellant 1 : {{ selectedRocketDetails.engines.propellant_1 }}</p>
      <p>Propellant 2 : {{ selectedRocketDetails.engines.propellant_2 }}</p>
      <p>Rocket Type : {{ selectedRocketDetails.engines.type }}</p>
      <p>Cost : {{ selectedRocketDetails.cost_per_launch }} $</p>
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
          this.selectedRocket = this.rockets[0][0]; 
          this.getRocketDetails(); 
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
.component-container1 {
  background-color: #f2f2f2;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  margin-right: 10px;
  width: 330px; /* Largeur souhaitée */
}
</style>
