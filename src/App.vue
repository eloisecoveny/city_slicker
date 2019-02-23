<template lang="html">
  <div class="">
    <h1>Select an urban area</h1>
    <div class="main-container">
      <urban-areas-list :urbanAreas="urbanAreas"></urban-areas-list>
      <urban-area-data :urbanArea="selectedUrbanArea" :continent="continent" :country="country" :image="image" :scores="scores"></urban-area-data>
    </div>
  </div>
</template>

<script>
import UrbanAreaList from './components/UrbanAreaList.vue';
import UrbanAreaData from './components/UrbanAreaData.vue';
import {eventBus} from './main.js';

export default {
  name: 'app',
  data(){
    return {
      urbanAreas: [],
      selectedUrbanArea: null,
      continent: "",
      country: "",
      image: "",
      scores: ""
      }
    },
  components: {
    "urban-areas-list": UrbanAreaList,
    "urban-area-data": UrbanAreaData
  },
  mounted(){
    fetch("https://api.teleport.org/api/urban_areas/")
      .then(response => response.json())
      .then(data => this.urbanAreas = data["_links"]["ua:item"])

      eventBus.$on('selected-urban-area', (urbanArea) => {
      this.selectedUrbanArea = urbanArea;
      fetch(this.selectedUrbanArea["href"])
        .then(response => response.json())
        .then(data => {
          this.continent = data.continent;
          this.country = data["_links"]["ua:countries"][0]["name"];
          this.getImage(data);
          this.getScores(data);
          });
      });
  },
  methods: {
    getImage(data){
      fetch(data["_links"]["ua:images"]["href"])
        .then(response => response.json())
        .then(images => this.image = images.photos[0]["image"]["web"]);
    },
    getScores(data){
      fetch(data["_links"]["ua:scores"]["href"])
        .then(response => response.json())
        .then(scores => this.scores = scores.categories);
    }
  }
}
</script>

<style lang="css" scoped>
</style>
