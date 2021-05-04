<template>
  <div>
    <div><the-header /></div>
    <div class="background">
      <img :src="backgroundImg" width="100%" height="100%" alt="" />
    </div>
    <div class="front">
      <div class="pic" @click="openImg">
        <input v-show="false" type="file" accept="image/jpeg" @change="tirggerFile($event)" ref="input" />
        <span v-if="imgUrl==''">点击上传</span>
        <img style="height:100%;width:100%;" v-if="imgUrl!=''" :src="imgUrl" />
      </div>
      <div style="margin-top: 30px;">
        <el-input style="width:350px;" :placeholder=newNickName v-model="newNickName" clearable maxlength="10"
          show-word-limit>
          <template slot="prepend">昵称:</template>
        </el-input>
      </div>
      <div style="margin-top: 30px;margin-left: 140px;">
        <el-button type="success" @click="submit()">提交</el-button>
      </div>
    </div>
  </div>
</template>

<script>
  import TheHeader from '../components/TheHeader.vue'
  export default {
    name:'pim',
    data() {
      return {
        id: '',
        imgUrl: '',
        newNickName: '',
        file_list: [],
        backgroundImg: require('../../static/image/1.jpg'),
        //post_url: 'http://192.168.43.235:8081',
      };
    },
    components:{
      TheHeader
    },
    mounted(){
      this.initialization();
    },
    methods: {
      initialization(){
        if(this.$route.params.id){
          this.id=this.$route.params.id;
        }
        if(this.$route.params.imgUrl){
          this.imgUrl=this.$route.params.imgUrl;
        }
        if(this.$route.params.nickName){
          this.newNickName=this.$route.params.nickName;
        }
      },
      openImg() {
        this.$refs.input.click();
      },
      tirggerFile: function(event) {
        this.file_list = [];
        let file = event.target.files[0];
        let url = "";
        var reader = new FileReader();
        reader.readAsDataURL(file);
        let that = this;
        reader.onload = function(e) {
          url = this.result.substring(this.result.indexOf(",") + 1);
          that.imgUrl = "data:image/png;base64," + url;
        };
        this.file_list = file;
      },
      submit() {
        let param = new FormData(); //创建form对象
        param.append('uid', this.id);
        param.append('uname', this.newNickName);
        param.append('head', this.file_list);
        this.$axios({
          headers: {
            "Content-Type": 'multipart/form-data'
          },
          url:'/alter/user',
          method: 'post',
          data: param
        }).then(response => {
          sessionStorage.clear();
          this.$store.dispatch("asyncUpdateUser", {
            name: this.newNickName,
            head: this.imgUrl,
            uid:this.id,
          })
          sessionStorage.setItem('state', JSON.stringify(this.$store.state));
          this.$message({
            type: 'success',
            message: '修改成功'
          })
          setTimeout(() => {
            this.$router.push("/PersonalCenter")
          }, 2000);
        }).catch(error => {
          this.$message({
            type: "error",
            message: "上传失败"
          })
        })
      }
    }
  }
</script>

<style scoped>
  .background {
    width: 100%;
    height: 95.3%;
    z-index: -1;
    position: absolute;
  }

  .front {
    z-index: 1;
    display: inline-block;
    margin-left:850px;
  }

  .pic {
    margin-top: 180px;
    display: inline-block;
    width: 178px;
    height: 178px;
    background-color: #ffffff;
    cursor: pointer;
    margin-left: 85px;
  }

  .pic span {
    margin-left: 60px;
    font-size: 15px;
    color: #55aaff;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
</style>
