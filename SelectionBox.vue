<template>
  <div class='select'>
    <el-dropdown>
      <el-button type="primary" class="optionButton" @click = "submit1">
        动漫
      </el-button>
      <el-dropdown-menu slot="dropdown" v-show = "false">
        <el-dropdown-item v-show = "false">国产动漫</el-dropdown-item>
        <el-dropdown-item v-show = "false">日本动漫</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

    <el-dropdown>
      <el-button type="primary" class="optionButton" @click = "submit2">
        音乐
      </el-button>
      <el-dropdown-menu slot="dropdown" v-show = "false">
        <el-dropdown-item v-show = "false">华语</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

    <el-dropdown>
      <el-button type="primary" class="optionButton" @click = "submit3">
      生活
      </el-button>
      <el-dropdown-menu slot="dropdown" v-show = "false">
        <el-dropdown-item v-show = "false">搞笑</el-dropdown-item>
        <el-dropdown-item v-show = "false">日常</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

    <el-dropdown>
      <el-button type="primary" class="optionButton" @click = "submit4">
        学习
      </el-button>
      <el-dropdown-menu slot="dropdown" v-show = "false">
        <el-dropdown-item v-show = "false">综艺</el-dropdown-item>
        <el-dropdown-item v-show = "false">明星</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

    <el-dropdown>
      <el-button type="primary" class="optionButton" @click = "submit5">
        美食
      </el-button>
      <el-dropdown-menu slot="dropdown" v-show = "false">
        <el-dropdown-item v-show = "false">单机游戏</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>

  </div>
</template>

<script>
  import axios from 'axios'
  import Qs from 'qs'
  export default {
    name: "selectionBox",
    data(){
      return{
        form:{
          type:''
        }
      }
    },
    methods:{
      submit1(){
        this.form.type = '动漫'
        this.$axios({
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            },
            method: 'post',
            url: '/search/type',
            data: Qs.stringify(this.form)
          }).then(res => {
             console.log(res.data)
             this.$router.push({
               name:'white',
               params:{
                 videoList: res.data,
                 title:this.form.type,
                 method:'类型'
               }
             })
          })
      },

      submit2(){
        this.form.type = '音乐'
        this.$axios({
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            },
            method: 'post',
            url: '/search/type',
            data: Qs.stringify(this.form)
          }).then(res => {
             console.log(res.data)
             this.$router.push({
               name:'white',
               params:{
                 videoList: res.data,
                 title:this.form.type,
                 method:'类型'
               }
             })
          })
      },

      submit3(){
        this.form.type = '生活'
        this.$axios({
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            },
            method: 'post',
            url: '/search/type',
            data: Qs.stringify(this.form)
          }).then(res => {
             console.log(res.data)
             this.$router.push({
               name:'white',
               params:{
                 videoList: res.data,
                 title:this.form.type,
                 method:'类型'
               }
             })
          })
      },

      submit4(){
        this.form.type = '学习'
        this.$axios({
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            },
            method: 'post',
            url: '/search/type',
            data: Qs.stringify(this.form)
          }).then(res => {
             console.log(res.data)
             this.$router.push({
               name:'white',
               params:{
                 videoList: res.data,
                 title:this.form.type,
                 method:'类型'
               }
             })
          })
      },

      submit5(){
        this.form.type = '美食'
        this.$axios({
            headers: {
              'Content-Type': 'application/x-www-form-urlencoded'
            },
            method: 'post',
            url: '/search/type',
            data: Qs.stringify(this.form)
          }).then(res => {
             console.log(res.data)
             this.$router.push({
               name:'white',
               params:{
                 videoList: res.data,
                 title:this.form.type,
                 method:'类型'
               }
             })
          })
      },
    }
  }
</script>

<style scoped>
  .select {
    position: relative;
    display: inline-block;
    width: 50%;
    height: 40px;
    top: -6px;
    left:520px;
  }

  .optionButton {
    display: inline-block;
    margin-left: 25px;
    background-color: unset;
    color: black;
    border: none;
    font-family: "微软雅黑";
    font-weight: 700;
    font-size: 20px;
  }
</style>
