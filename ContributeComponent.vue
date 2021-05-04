<template>
  <div class="ContributeComponent">
    <el-form ref="form" :model="form" :rules="rules">
      <div style="margin-top: 50px;">
        <h3 style="text-align: center;">投稿内容</h3>
      </div>

      <div style="margin-top: 30px;margin-left: 200px;">
        <h4>基本信息</h4>
      </div>

      <div style="margin-left: 200px;">
        <el-upload action="" class="upload-demo" :limit="1" drag accept=".mp4" multiple :auto-upload="false"
          name="file" :on-change="changeVideo">
          <i class="el-icon-upload"></i>
          <div class="el-upload__text" v-show="hide_visible1">上传一个视频文件，大小不超过10MB</div>
          <div class="el-upload__text" v-show="hide_visible">视频已选择</div>
        </el-upload>
      </div>

      <div style="margin-left: 200px; margin-top: 30px;" v-show="hide_visible">
        <div>
          <h5 style="color: blue">*视频标题</h5>
        </div>
        <div><input type="text" style="width: 700px; height: 30px; border: 0px; outline: none;" v-model="form.title">
        </div>
      </div>

      <div style="margin-left: 200px; margin-top: 30px;" v-show="hide_visible">
        <div>
          <h5 style="color: blue">*视频封面</h5>
        </div>
        <el-upload action="" class="upload-demo" :limit="1" drag accept=".jpg,.png,.jpeg" multiple
          :auto-upload="false" name="file" :on-change="changeImage" :name="cover">
          <i class="el-icon-upload"></i>
          <div class="el-upload__text" v-show="hide_visible3">上传该视频的封面,大小不超过1MB</div>
          <div class="el-upload__text" v-show="hide_visible2">封面已选择</div>
        </el-upload>
      </div>

      <div style="margin-left: 200px; margin-top: 30px;" v-show="hide_visible">
        <div>
          <h5 style="color: blue">*视频类型</h5>
        </div>
        <div>
          <el-radio-group v-model="form.type">
            <el-radio-button label="学习"></el-radio-button>
            <el-radio-button label="美食"></el-radio-button>
            <el-radio-button label="生活"></el-radio-button>
            <el-radio-button label="音乐"></el-radio-button>
            <el-radio-button label="动漫"></el-radio-button>
          </el-radio-group>
        </div>
      </div>


      <div style="margin-left: 200px; margin-top: 30px;" v-show="hide_visible">
        <div>
          <h5 style="color: blue">*视频简介</h5>
        </div>
        <div><textarea v-model="form.introduction"
            style="width: 700px;height: 100px; border: 0px; outline: none"></textarea></div>
      </div>

      <div style="margin-left: 200px; margin-top: 30px;" v-show="hide_visible">
        <button type="button" @click="submitAll('form')"
          style="width: 100px; height: 30px; background-color: skyblue; color: white; margin-left: 300px; text-align: center;">提交</button>
      </div>

    </el-form>
  </div>
</template>

<script>
  import axios from 'axios'
  import Qs from 'qs'
  export default {
    name: "contributecomponent",
    data() {
      return {
        hide_visible: false,
        hide_visible1: true,
        hide_visible2: false,
        hide_visible3: true,

        form: {
          title: '',
          introduction: '',
          type:'学习',
          coverFile: [],
          videoFile: [],
        },
        rules: {
          title: [{
            required: true,
            message: '请输入标题',
            trigger: 'blur'
          }],
          introduction: [{
            required: true,
            message: '请输入视频简介',
            trigger: 'blur'
          }]
        }
      }

    },

    methods: {
      changeVideo(file, fileList) {
        const isVedio = file.raw.type === "video/mp4"
        const isLt10M = file.raw.size / 1024 / 1024 < 10;

        if (!isVedio) {
          this.$message.error('上传文件只能是视频格式!');
          fileList.splice(0, 1) //如果格式不对的文件，直接强制移除
          return;
        } else if (!isLt10M) {
          this.$message.error('上传文件大小不能超过10MB!');
          fileList.splice(0, 1) //如果格式不对的文件，直接强制移除
          return;
        }

        if (fileList.length == 1) {
          this.$message.success('视频已上传')
          this.hide_visible = true
          this.hide_visible1 = false
          console.log(file)
          this.form.videoFile = file.raw
        }

        if (fileList.length == 0) {
          this.hide_visible = false
          this.hide_visible1 = true
        }
      },

      changeImage(file, fileList) {
        const isImage = (file.raw.type === "image/png") || (file.raw.type === "image/jpg") || (file.raw.type ===
          "image/jpeg")
        const isLt1M = file.raw.size / 1024 / 1024 < 1;

        if (!isImage) {
          this.$message.error('上传文件只能是图像格式!');
          fileList.splice(0, 1) //如果格式不对的文件，直接强制移除
          return;
        } else if (!isLt1M) {
          this.$message.error('上传文件大小不能超过1MB!');
          fileList.splice(0, 1) //如果格式不对的文件，直接强制移除
          return;
        }

        if (fileList.length == 1) {
          this.$message.success('封面已上传')
          this.hide_visible2 = true
          this.hide_visible3 = false
          console.log(file.raw)
          this.form.coverFile = file.raw
        }

        if (fileList.length == 0) {
          this.hide_visible2 = false
          this.hide_visible3 = true
        }
      },

      submitAll(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            let Form = new FormData();
            console.log(this.form.coverFile)
            Form.append("uid", this.$store.getters.getUser.uid);
            Form.append("title", this.form.title);
            Form.append("type", this.form.type);
            Form.append("introduction", this.form.introduction);
            Form.append("cover", this.form.coverFile);
            Form.append("video", this.form.videoFile);

            if(this.form.coverFile.length == 0){
              this.$message({
                type: 'error',
                message: '请添加视频封面!'
              });
              return false;
            }

            this.$axios({
              headers: {
                'Content-Type': 'multipart/form-data'
                //'Content-Type': 'application/x-www-form-urlencoded'
              },
              method: 'post',
              url: "/upload/video",
              data: Form,
            }).then(res => {
              console.log(res.data)
              this.$message({
                type: 'success',
                message: '提交成功'
              });
            })
          } else {
            this.$message({
              type: 'warning',
              message: '请输入标题或视频简介'
            });
            return false;
          }
        })
      }

    },



  }
</script>

<style lang="scss" scoped>
  .ContributeComponent {
    background-color: #69e6ff;
    position: absolute;
    margin-left: 20%;
    height: 1050px;
    width: 60%;
  }
</style>
