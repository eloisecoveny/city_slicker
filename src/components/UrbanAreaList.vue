<template lang="html">
  <div>
    <select v-on:change="handleChange" v-model="selectedUrbanAreaIndex">
      <option v-for="(urbanArea, index) in urbanAreas" :value="index">{{ urbanArea.name }}</option>
    </select>
  </div>
</template>

<script>
import {eventBus} from '../main.js';

export default {
  name: 'urban-areas-list',
  props: ['urbanAreas'],
  data() {
    return {
      selectedUrbanAreaIndex: null
    }
  },
  methods: {
    handleChange() {
      eventBus.$emit('selected-urban-area', this.urbanAreas[this.selectedUrbanAreaIndex]);
    }
  }
}
</script>

<style lang="css" scoped>
</style>
