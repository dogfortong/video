<template>
  <div class="header">
    <div class="logo" @click="goHome">
      <i class="el-icon-video-camera-solid" style="font-size:30px; color: deeppink;"></i>
      <span class="word" style="font-size:30px;color: deeppink;">Video</span>
    </div>

    <selectionBox />

    <div class="search-type">
      <el-radio-group v-model="type">
        <el-radio-button label="搜用户"></el-radio-button>
        <el-radio-button label="搜视频"></el-radio-button>
      </el-radio-group>
    </div>

    <div class="search">
      <input type="text" placeholder="请输入您要搜索的内容..." v-model="searchWord" /><button type="button" @click="search"><i class="el-icon-search"
          style="cursor: pointer;"></i></button>
    </div>

    <div class="admin">
      <el-container class="El-container">
        <el-dropdown>
          <img class="adminPic" :src = "this.$store.getters.getUser.head ? this.$store.getters.getUser.head : '../../static/image/1.jfif'">
          <el-dropdown-menu slot="dropdown" class="adminchoice">
            <span v-if="$store.getters.getUser.name === ''">
              <el-link type=" primary" href="/login">未登录</el-link>
            </span>
            <span class="username" v-else>
              {{$store.getters.getUser.name}}
            </span>
            <el-dropdown-item @click.native = "gotoper">个人中心</el-dropdown-item>
            <el-dropdown-item @click.native = "goPage('/notification',' ')">未读消息</el-dropdown-item>
            <el-dropdown-item @click.native = "goPage('/contribution',' ')">投稿管理</el-dropdown-item>
            <el-dropdown-item @click.native="exit($store.getters.getUser.name)">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-container>
    </div>
    <button class="ContributionBtn" @click="Contribution">投稿</button>
  </div>
</template>

<script>
  import axios from 'axios'
  import Qs from 'qs'
  import SelectionBox from '../components/SelectionBox.vue'
  export default {
    name: 'the-header',
    data(){
      return{
        type:'搜用户',
        searchWord:'',
        userform:{
          uname:''
        },
        videoform:{
          title:''
        }
      }
    },
    components: {
      SelectionBox
    },
    methods: {
      goHome() {
        this.$router.push({
          path: '/'
        });
      },

      goPage(path, name) {
        this.$router.push({
          path: path
        });
      },

      exit(name) {
        if (name == '') {
          this.$message({
            type: 'warning',
            message: '您当前未登录',
            duration: '1000'
          });
        } else {
          console.log(this.$store.getters.getUser.head);
          sessionStorage.clear();
          this.$message({
            type: 'warning',
            message: '登出成功！',
            duration: '2000'
          });
          setTimeout(() => {
            window.location.reload();
          }, 2000);
        }
      },

      Contribution() {
        this.$router.push({
          path: '/contribution'
        })
      },

      gotoper(){
        console.log("1111");
        this.$router.push({
          path:'/personalcenter'
        })
      },

      search(){
        if(this.type =="搜视频"){
           console.log("开始搜视频")
           this.videoform.title=this.searchWord
           this.$axios({
               headers: {
                 'Content-Type': 'application/x-www-form-urlencoded'
               },
               method: 'post',
               url: '/search/video',
               data: Qs.stringify(this.videoform)
             }).then((response) => {
               if(response.data.length == 0){
                 this.$message({
                   type: 'error',
                   message: '没有搜索到相关视频'
                 });
               }else{
                 console.log(response.data)
                 this.$router.push({
                   name:'white',
                   params:{
                     videoList:response.data,
                     title:this.searchWord,
                     method:'标题'
                   }
                 })
               }
             })
        }

        if(this.type =="搜用户"){
           console.log("开始搜用户")
           this.userform.uname = this.searchWord
           this.$axios({
               headers: {
                 'Content-Type': 'application/x-www-form-urlencoded'
               },
               method: 'post',
               url: '/search/user',
               data: Qs.stringify(this.userform)
             }).then((response) => {
                //console.log(response.data)
                if(response.data.result == "1"){
                  console.log(response.data.uid)
                  this.$router.push({
                    name:'authorHome',
                    params:{
                      uid:response.data.uid
                    }
                  })
                }
                else{
                  this.$message({
                    type: 'error',
                    message: '没有搜索到此用户'
                  });
                }
             })
        }

      }

    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
  }

  .header {
    display: inline-block;
    margin-top: 0px;
    background: url("../../static/image/9.jpg");
    background-size: 100% 100%;
    width: 100%;
    height: 50px;
    left: 0px;
    top: 0px;
  }

  .logo {
    position: relative;
    display: inline-block;
    cursor: pointer;
    top: -6px;
    left: 200px;
  }

  .search-type{
    position: relative;
    display: inline-block;
    width: 180px;
    height: 30px;
    top: -10px;
    left: 230px;
  }

  .search {
    position: relative;
    display: inline-block;
    width: 260px;
    height: 50px;
    top: -15px;
    left: 220px;
  }

  .search input {
    border: 0px;
    outline: none;
    color: #9E9C9C;
    width: 165px;
    height: 40px;
    vertical-align: top;
  }

  .search button {
    border: 0px;
    outline: none;
    background: #7BA7AB;
    color: white;
    width: 50px;
    height: 40px;
    vertical-align: top;
    /* border-radius: 0 5px 5px 0; */
  }

  /* .admin {
    position: relative;
    display: inline-block;
    height: 50px;
    top: 0px;
    left: 500px;
  } */

  .header-body {
    display: inline-block;
    font: 20px verdana, arial, sans-serif;
    height: 50px
  }

  .header-body li {
    display: inline-block;
    width: 100px;
  }

  .admin {
    display: inline-block;
    position: relative;
    width: 50px;
    height: 50px;
    top: 5px;
    left: 200px;
  }

  .adminPic {
    display: inline-block;
    width: 40px;
    height: 40px;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.6s;
  }

  .adminPic:hover {
    transform: scale(1.4);
  }

  .username {
    text-align: center;
  }

  .adminchoice {
    display: inline-block;
    text-align: center;
  }

  .ContributionBtn {
    position: relative;
    display: inline-block;
    background-color: deeppink;
    color: white;
    width: 100px;
    height: 45px;
    outline: none;
    border: white;
    top: -8px;
    left: 210px;
  }
</style>
