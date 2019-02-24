<template lang="html">
  <div id="flex-container">
    <select v-on:change="handleChange" v-model="selectedUrbanAreaIndex">
      <option disabled value="">Select an urban area</option>
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

#flex-container {
  display: flex;
  justify-content: center;
}

select {
  margin: 30px;
  background-color: White;
  color: black;
}

select:active, select:hover {
  outline-color: none;
}

</style>
