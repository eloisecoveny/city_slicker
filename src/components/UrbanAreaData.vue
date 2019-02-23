<template lang="html">
  <div v-if="urbanArea">
    <h1>{{ urbanArea.name }}</h1>
    <p>{{ country }}, {{ continent }}</p>
    {{ generateChartData }}
    <GChart
    v-if="chartData"
    type="BarChart"
    :data="chartData"
    />
    <img :src="image">
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
  computed: {
    generateChartData(){
      const data = [];
      data.push(this.scores.map(heading => heading["name"]));
      data.push(this.scores.map(score => score["score_out_of_10"]))
      this.chartData = data;
    },
  },
  methods: {

  }
}
</script>

<style lang="css" scoped>
</style>
