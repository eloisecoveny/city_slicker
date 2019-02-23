<template lang="html">
  <div v-if="urbanArea">
    <h1>{{ urbanArea.name }}</h1>
    <p>{{ country }}, {{ continent }}</p>
    <img :src="image">
    <GChart
    v-if="chartData"
    type="ComboChart"
    :data="chartData"
    :options="chartOptions"
    />
  </div>

</template>

<script>
import { GChart } from 'vue-google-charts';

export default {
  name: 'urban-area-data',
  props: ['urbanArea', 'continent', 'country', 'image', 'scores'],
  components: {
    "gchart": GChart
  },
  data(){
    return {
      chartData: [],
      chartOptions: {
      }
    }
  },
  methods: {
    generateChartData(){
      const data = [];
      for(let name of this.scores){
        data.push(Object.values(name["name"]));
      };
      for(let score of this.scores){
        data.push(Object.values(score["score_out_of_10"]))
      };
      this.chartData = data;
    }
  }
}
</script>

<style lang="css" scoped>
</style>
