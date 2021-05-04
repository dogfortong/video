<template>
  <div>
    <div class="background">
      <img :src="backgroundImg" width="100%" height="100%" alt="">
    </div>
    <div class="front">
      <div class="avatarPic">
        <div class="avatar_img">
          <img style="width:60px;height:60px;border-radius:50%;cursor:pointer;float:left;margin-top:12px;margin-left:80px;" :src="avatar">
        </div>
        <div class="avatar_info" @click="modifyAvatar()">
          <p style="color:white; font-size: 5px; line-height: 60px;">更换头像</p>
        </div>
      </div>
      <div style="display:inline-block;font-size:22px;float:left;margin-top:15px;margin-left:150px;margin-bottom: 50px;">
        <span>{{nickName}}</span>
      </div>
      <div>
        <el-row>
          <el-col :span="22" :offset="1">
            <el-tabs type="border-card" v-model="activeName" @tab-click="handleClick">
              <el-tab-pane name="first">
                <span slot="label"><i class="el-icon-video-camera-solid"></i>我的视频</span>
                <el-container style="height: 520px; border: 0px solid #eee">
                  <el-main>
                    <p style="font-size: 20px;margin-left: 700px;margin-top: 200px;" v-show="visible">你还未发布视频哦！快去发布吧！</p>
                    <div class="videoline">
                      <div v-for="(item,index) in myvideo" :key="index" @click="gotoVideoPlayer()"
                        class="videoline-div">
                        <div class="singlePic">
                          <div class="maskImage">
                            <img :src="item.cover">
                            <div class="message">
                              <p style="color: white;font-size:5px;">
                                {{item.ptime}}&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp{{item.duration}}
                              </p>
                            </div>
                          </div>
                          <div class="info" @click="gotoVideoPlayer()">
                            <!-- <p style="color:white;height:50px;font-size:15px;">[视频概述]</p> -->
                            <h5 style="color:white;line-height:120px;height:130px;">{{item.introduction}}</h5>
                            <div class="imageIcon">
                              <i class="el-icon-video-play" style="color: white;" @click="gotoVideoPlayer()"></i>
                            </div>
                          </div>
                        </div>
                        <div class="text">
                          <p style="line-height:30px;color:black;">{{item.title}}</p>
                          <!-- <p style="line-height:0px;color:black;">[视频{{index+1}}] {{item.title}}</p> -->
                          <!-- <p style="line-height:15px;color:gray;font-size:10px;">{{item.uname}}</p> -->
                        </div>
                      </div>
                    </div>
                  </el-main>
                </el-container>
              </el-tab-pane>
              <el-tab-pane name="second">
                <span slot="label"><i class="el-icon-star-off"></i>我的收藏</span>
                <el-container style="height: 520px; border: 0px solid #eee">
                  <el-main>
                    <p v-show="visible1" style="font-size: 20px;margin-left: 700px;margin-top: 200px;">你还未收藏视频哦！快去收藏视频吧！</p>
                    <div class="videoline">
                      <div v-for="(item,index) in mycollection" :key="index" @click="gotoVideoPlayer()"
                        class="videoline-div">
                        <div class="singlePic">
                          <div class="maskImage">
                            <img :src="item.cover">
                            <div class="message">
                              <p style="color: white;font-size:5px;">
                                {{item.ptime}}&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp{{item.duration}}
                              </p>
                            </div>
                          </div>
                          <div class="info" @click="gotoVideoPlayer()">
                            <h5 style="color:white;line-height:120px;height:130px;">{{item.introduction}}</h5>
                            <div class="imageIcon">
                              <i class="el-icon-video-play" style="color: white;" @click="gotoVideoPlayer()"></i>
                            </div>
                          </div>
                        </div>
                        <div class="text">
                          <p style="line-height:30px;color:black;">{{item.title}}</p>
                        </div>
                      </div>
                    </div>
                  </el-main>
                </el-container>
              </el-tab-pane>
            </el-tabs>
          </el-col>
        </el-row>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name:'pcc',
    data() {
      return {
        id: this.$store.getters.getUser.uid,
        //id: this.$router.params.uid,
        avatar: '',
        visible:false,
        visible1:false,
        //avatar: require('../assets/avatar.jpg'),
        nickName: '',
        activeName: 'first',
        backgroundImg: require('../../static/image/1.jpg'),
        //post_url: 'http://192.168.43.229:8081',
        myvideo: [],
        mycollection: [],
      };
    },
    created() {
      this.userData();
      this.videoData();
      this.collectData();
    },
    methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      },
      modifyAvatar() {

        this.$router.push({
          name: 'pim',
          params: {
            id: this.id,
            imgUrl: this.avatar,
            nickName: this.nickName
          }
        })
      },
      gotoVideoPlayer() {
        // this.$router.push({
        //   path: '/videoplayer'
        // })
      },
      userData() {
        this.$axios({
          url: '/user/' + this.id,
          method: 'get',
        }).then(response => {
          console.log(response.data);
          this.avatar = response.data.avatar;
          this.nickName = response.data.nickName;
        }).catch(error => {
          console.log(response);
        })
      },
      videoData() {
        this.$axios({
          url:'/myvideo/' + this.id,
          method: 'get',
        }).then(response => {
          console.log(response.data);
          this.myvideo = response.data;
          if(response.data.length == 0) {
            this.visible = true
          }
        }).catch(error => {
          console.log(response);
        })
      },
      collectData() {
        this.$axios({
          url:'/mycollection/' + this.id,
          method: 'get',
        }).then(response => {
          console.log(response.data);
          this.mycollection = response.data;
          if(response.data.length == 0) {
            this.visible1 = true
          }
        }).catch(error => {
          console.log(response);
        })
      }
    }
  };
</script>

<style scoped>
  .background {
    width: 99%;
    height: 93%;
    z-index: -1;
    position: absolute;
  }

  .front {
    z-index: 1;
  }

  .el-tabs__item {
    font-size: 20px !important;
  }

  .el-main {}

  .el-main::-webkit-scrollbar {
    display: none;
  }

  .videoline {
    display: grid;
    grid-template-columns: repeat(auto-fill, 20%);
    padding: 10px;
    /* grid-column-gap: 5%; */
    grid-row-gap: 100px;
  }

  .videoline-div {
    /* display: block; */
    width: 80%;
    height: 120px;
    margin-left: 10%;
    cursor: pointer;
  }

  .avatarPic {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .avatar_img {
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 1;
  }

  .avatar_info {
    position: absolute;
    width:60px;
    height:60px;
    border-radius:50%;
    cursor: pointer;
    margin-left: 80px;
    margin-top: 12px;
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

  .avatarPic:hover .avatar_info {
    opacity: 1;
    /*有opacity有0变成1*/
  }

  .singlePic {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .singlePic img {
    width: 100%;
    height: 100%;
    cursor: pointer;
    transition: all 0.6s;
    float: left;
  }

  .maskImage {
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 1;
  }

  .message {
    width: 100%;
    height: 30%;
    position: absolute;
    margin-top: 95px;
  }

  .info {
    position: absolute;
    width: 100%;
    height: 100%;
    cursor: pointer;
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
    left: 130px;
    top: -50px;
  }

  .text {
    width: auto;
    cursor: auto;
    text-align: left;
  }
</style>
