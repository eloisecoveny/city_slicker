<template lang="html">
  <div id="flex-container">
    <link href="https://fonts.googleapis.com/css?family=Nova+Round|Nanum+Gothic" rel="stylesheet">
    <h1>City Slicker</h1>
    <nav>
      <div id="urban-areas-list">
        <urban-areas-list :urbanAreas="urbanAreas"></urban-areas-list>
      </div>

      <div id="category-list">
        <category-list :categories="categories"></category-list>
      </div>
    </nav>

    <div id="urban-area-data">
      <urban-area-data :urbanArea="selectedUrbanArea" :continent="continent" :country="country" :image="image" :scores="scores"></urban-area-data>
    </div>

  </div>
</template>

<script>
import UrbanAreaList from './components/UrbanAreaList.vue';
import UrbanAreaData from './components/UrbanAreaData.vue';
import CategoryList from './components/CategoryList.vue';
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
      selectedCategoryIndex: null,
      categories: ["Housing", "Cost of Living", "Startups", "Venture Capital", "Travel Connectivity", "Commute", "Business Freedom", "Safety", "Healthcare", "Education", "Environmental Quality", "Economy", "Taxation", "Internet Access", "Leisure & Culture", "Tolerance", "Outdoors"],
      categoryTopTen: [],
      categoryScores: []
    }
  },
  components: {
    "urban-areas-list": UrbanAreaList,
    "urban-area-data": UrbanAreaData,
    "category-list": CategoryList
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

    eventBus.$on('selected-category', (categoryIndex) => {
      this.selectedCategoryIndex = categoryIndex;
      this.getCategoryScores(categoryIndex);
      this.getTopTen();
    })
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
    },
    getCategoryScores(categoryIndex){
      const scores = []
      this.urbanAreas.forEach(urbanArea => {
        fetch(urbanArea["href"])
        .then(response => response.json())
        .then(data => fetch(data["_links"]["ua:scores"]["href"]))
        .then(response => response.json())
        .then(values => {
          let score = values.categories[categoryIndex]["score_out_of_10"]
          scores.push({name: urbanArea.name, score: score})
        });
      }, [0]);
      this.categoryScores = scores;
    },
    getTopTen(){
      let sortedScores = this.categoryScores.sort((a, b) => {
        return b.score - a.score;
      });
      this.categoryTopTen = sortedScores.slice(0, 10);
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

nav {
  display: flex;
  flex-direction: row;
  justify-content: center;
  width: 100%;
}

h1 {
  font-family: 'Nova Round', cursive;
}

</style>
