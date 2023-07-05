<script setup>
import RocketComponent from './components/RocketComponent.vue';
import Lauchpad from './components/Lauchpads.vue';
import DashboardTitle from './components/DashBoardTitle.vue'
import Footer from './components/Footer.vue'
import Graph from './components/PieGraph.vue'
import GraphBar from './components/GraphBar.vue'
import GoogleMap from "./components/GoogleMap.vue";
import Landpad from "./components/Landpads.vue"
import Mission from "./components/Mission.vue"
import Picture from "./components/Picture.vue"







</script>

<template>
  <header>
    <DashboardTitle />
  </header>
  <main>

    <div class="background-container">
      <div class="container2 ">
        <RocketComponent />
        <GraphBar />
        <Graph />
        <Picture />
      </div>
      <br>
      <div class="container2 ">
        <Mission />
        <Lauchpad />
        <GoogleMap />

        <Landpad />
      </div>
    </div>




  </main>

  <footer>
    <Footer />
  </footer>
</template> 



<style>
.background-container {
  background: #c9c7c7;
  margin-top: 10px;
  margin-left: 10px;
  margin-right: 10px;
  padding: 10px;
  border-radius: 10px;
  box-sizing: border-box;
  width: auto;
  height: 815px;

}

.graph {
  width: 30%;
}

.container2 {

  width: 100%;
  height: auto;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  flex-flow: wrap;
}
</style>
