<template>
  <div class="HomeList">
    <ul>
      <li class="VideoList" v-for="(item,index) in HomeList" :key="index">
        <div class="singlePic">
          <div class="maskImage">
            <img :src="item.cover" />
            <div class = "message">
             <h6 style="color: white;">{{item.ptime}}&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp{{item.duration}}</h6>
            </div>
            <div class = "message1">
             <h3 style="color: black;">[视频名] {{item.title}}</h3>
             <h5 style="color: gray;">[作者] {{item.uname}}</h5>
            </div>
          </div>
          <div class="info" @click="gotoVideoPlayer(item.vid)">
            <h3 style="color: white; width: 100%;height: 80px;"></h3>
            <h6 style="color: white; width: 100%; height: 150px;">{{item.introduction}}</h6>
            <div class="imageIcon">
              <i class="el-icon-video-play" style="color: white;" @click="gotoVideoPlayer(item.vid)"></i>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
  import {
    HomeList
  } from '../assets/HomeVideos.js'
  import Qs from 'qs'
  import axios from 'axios'
  export default {
    name: "Homelist",
    data() {
      return {
        HomeList: []
      }
    },
    created() {
      this.$axios({
          headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          },
          method: 'post',
          url: '/exhibit',
          data: Qs.stringify(this.loginForm)
        })
        .then((response) => {
             this.HomeList = response.data;
             console.log(this.HomeList);
          })
    },
    methods: {
      gotoVideoPlayer(vid) {
        this.$router.push({
          path: '/play',
          query:{
            vid:vid,
            uid:this.$store.getters.getUser.uid
          }
        })
      }
    }
  }
</script>

<style scoped>
  .HomeList {
    display: inline-block;
    width:100%;
    /* margin-left: 10%; */
    height: 1500px;
    /* background-color: #409EFF; */
  }

  .VideoList {
    display: inline-block;
  }

  .singlePic {
    position: relative;
    display: inline-block;
    margin-left: 50px;
    margin-top: 50px;
    top: 30px;
    width: 320px;
    height: 180px;
  }

  .singlePic img {
    width: 100%;
    height: 100%;
    cursor: pointer;
    transition: all 0.6s;
    float:left;
  }

  .message{
    width: 100%;
    height: 30%;
    text-align: left;
    position: absolute;
    margin-top:160px;
  }

  .message1{
    width: 100%;
    height: 10%;
    text-align: left;
  }
  .maskImage {
    position: absolute;
    width: 320px;
    height: 180px;
    z-index: 1;
  }

  .info {
    position: absolute;
    width:100%;
    height:100%;
    cursor: pointer;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    text-align: center;
    -webkit-backface-visibility: hidden;
    /* 隐藏旋转元素的背面*/
    backface-visibility: hidden;
    background: rgba(0, 0, 0, 0.6);
    /*后面这个0.6是指的背景的透明度*/
    opacity: 0;
    -webkit-transition: all 0.35s ease-in-out;
    /*规定提示信息怎样出现ease-in-out以慢速度开始和结束*/
    -moz-transition: all 0.35s ease-in-out;
    transition: all 0.35s ease-in-out;
    z-index: 2;
  }

  .singlePic:hover .info {
    opacity: 1;
    /*有opacity有0变成1*/
  }

  .imageIcon {
    position: relative;
    font-size: 30px;
    left: 140px;
    top: -90px;
  }
</style>
