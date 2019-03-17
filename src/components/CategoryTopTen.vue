<template lang="html">
  <div id="flex-container" v-if="topTen">
    <h1>Category top ten</h1>
    <p>{{ selectedCategory }}</p>
  </div>

</template>

<script>
import {eventBus} from '../main.js';

export default {
  name: 'category-top-ten',
  props: ['topTen', 'selectedCategory','urbanAreas']
}
</script>

<style lang="css" scoped>
</style>
