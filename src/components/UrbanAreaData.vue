<template lang="html">
  <div v-if="urbanArea">
    <h1>{{ urbanArea.name }}</h1>
    <p>{{ country }}, {{ continent }}</p>
  </div>

</template>

<script>
export default {
  name: 'urban-area-data',
  props: ['urbanArea', 'continent', 'country', 'image', 'scores'],
  data(){
    return {
    }
  }
}
</script>

<style lang="css" scoped>
</style>
