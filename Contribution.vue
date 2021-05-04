<template>
  <div>
    <div>
      <the-header />
    </div>
    <div>
      <Contribution />
    </div>
  </div>
</template>

<script>
  import TheHeader from '../components/TheHeader.vue'
  import Contribution from '../components/ContributeComponent.vue'
  export default {
    name: 'contribution',
    components: {
      TheHeader,
      Contribution
    }
  }
</script>

<style>
</style>
