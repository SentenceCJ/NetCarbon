<template>
    <div class="component-container">
      <h3 style="text-align: center;">SpaceX Mission History</h3>
      <br>
      <select v-model="selectedEvent" @change="displayEventDetails">
        <option value="">Select an event</option>
        <option v-for="event in events" :value="event.id" :key="event.id">{{ event.title }}</option>
      </select>
      <div v-if="selectedEvent">
        <br>
        <p>Mission Name : {{ selectedEvent.title }}</p>
        <br>
        <p>Date & hour : {{ selectedEvent.event_date_utc }}</p>
        <!-- <p>{{ selectedEvent.details }}</p> -->
        <br>
        <a :href="selectedEvent.links.article">More infos..</a>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        events: [],
        selectedEvent: null,
      };
    },
    mounted() {
      this.fetchEvents();
    },
    methods: {
      async fetchEvents() {
        try {
          const response = await axios.get('https://api.spacexdata.com/v4/history');
          this.events = response.data;
          if (this.events.length > 0) {
            this.selectedEvent = this.events[0].id; // Sélectionne le premier événement par défaut
            this.displayEventDetails(); // Affiche les détails de l'événement sélectionné par défaut
          }
        } catch (error) {
          console.error(error);
        }
      },
      displayEventDetails() {
        if (this.selectedEvent) {
          this.selectedEvent = this.events.find(event => event.id === this.selectedEvent);
        }
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
  