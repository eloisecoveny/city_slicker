<template lang="html">
  <div id="flex-container">
    <div class="selection-field">
    <link href="https://fonts.googleapis.com/css?family=Nova+Round|Nanum+Gothic" rel="stylesheet">

    <h1>Select an urban area</h1>
      <urban-areas-list :urbanAreas="urbanAreas"></urban-areas-list>
    </div>

      <urban-area-data :urbanArea="selectedUrbanArea" :continent="continent" :country="country" :image="image" :scores="scores"></urban-area-data>
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
      scores: [],
      colors: []
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

#flex-container {
  display: flex;
  align-items: center;
  flex-direction: column;
}

h1 {
  font-family: 'Nova Round', cursive;
}

.selection-field {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px 80px;
  width: 90%;
  background-color: lightgrey;
  /* border: 3px solid; */
}

</style>
