<template>
  <div>
    <div><the-header /></div>
    <div><swiper /></div>
    <div>
      <span class = "main-title">首  页  推  荐</span>
    </div>
    <div><Homelist /></div>
  </div>
</template>


<script>
  import Swiper from "../components/Swiper.vue"
  import TheHeader from '../components/TheHeader.vue'
  import SelectionBox from '../components/SelectionBox.vue'
  import Homelist from '../components/HomeVideos.vue'
  import {navMsg} from '../assets/header.js'
  export default {
    name: 'Main',
    components: {
      TheHeader,
      Swiper,
      Homelist
    }
  }
</script>


<style lang="scss" scoped>
  .main-title{
    font-size:30px;
    text-align: center;
    margin-left:46%;
    margin-top:0px;
    color: blue;
    background-color: black;
  }

  .test {
    display: inline-block;
    height: 800px;
    width: 100%;
    background-color: #42B983;
  }
</style>
