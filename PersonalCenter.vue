<template>
  <div>
     <div><the-header /></div>
     <div><PCC /></div>
  </div>
</template>

<script>
  import TheHeader from '../components/TheHeader.vue'
  import PCC from '../components/PersonalCenterComponent.vue'
  export default {
    name: 'PersonalCenter',
    components:{
      TheHeader,
      PCC
    }
  }
</script>

<style>
</style>
