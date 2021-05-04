<template>
  <div class="isZan-container">
    <img :src=icon>
      <span @click="addLike()" :class="{'isLike':likeClass}" class="container-inner">
          <transition @before-enter="beforeEnter" @enter="enter"
                      @after-enter="afterEnter">
              <span class="addOne" v-if="isLike">+1</span>
              <span class="cancleOne" v-if="like">-1</span>
          </transition>
          <i :class="[icon,{'isLike':likeClass}]"></i>
         {{getLikeClass.title }}:{{getLikeClass.technicalListDetail.t_zan}}
      </span>
  </div>
</template>

<script>
export default {
  name: "isLike",
  data() {
    return {
      isLike: false, // 点赞+1
      like: false, // 点赞-1
      likeClass: "", //点赞后样式改变
      commentLike_id: "",//请求数据时点赞Id
      postLike: "",
      cancleLike: "",
      icon: '',
    }
  },
  props: ['getLikeClass'],

  methods: {
    //点击增加收藏和点赞（动画和样式）
    addLike() {
      //判断是点赞
      if (this.likeClass) { //说明已经点赞，这次是取消
        this.like = !this.like; //-1显示
        this.likeClass = !this.likeClass; //样式还原
        this.setLikeCount(); //请求点赞
      } else {
        this.isLike = !this.isLike; //说明未点赞，这次是点赞，显示+1
        this.likeClass = !this.likeClass;
        this.setLikeCount();//请求点赞
      }
    },
    //发送请求记录点赞
    setLikeCount: function () {
      if (this.isLike) { //如果是点赞 +1
        this.$http.post(this.postLike).then((response)=>{
          if (response.data !== null && response.data!=="") {
            this.$message({
              message: '谢谢您的点赞',
              type: 'success',
              duration: 800
            });
            this.$emit('getArticleDetail')
          } else {
            this.$message({
              message: '' + error + '，请先登录',
              type: 'error',
              duration: 500
            })
          }
        },(response)=>{
          this.$message({
            message: '' + response + '',
            type: 'error',
            duration: 500
          })
        });

      } else {
        //this.$alert("取消点赞 "+this.cancleLike+this.commentLike_id)
        this.$http.post(this.cancleLike+this.commentLike_id)
          .then((res) => {
            //console.log(res)
            res = res.data;
            if (res===true) {
              this.$message({
                message: '已取消点赞',
                type: 'success',
                duration: 800,
              });
              this.$emit('getArticleDetail')
            } else {
              this.$message({
                message: '' + error+ '',
                type: 'error',
                duration: 8
              })
            }
          });
      }
    },
    beforeEnter(el) {
      //动画开始进入前
      el.style.transform = "translate(0,0)"
    },
    enter(el, done) {
      // console.log(el);
      //动画执行时
      el.offsetWidth;
      el.style.transform = "translate(10px,-22px)";
      el.style.transition = "all 0.6s ease";
      done()
    },
    afterEnter(el) {
      //动画结束时
      //console.log(this.likeClass);
      if (this.likeClass) { // 如果是true动画出现的是+1，出现后再隐藏
        this.isLike = !this.isLike;
      } else {
        this.like = !this.like;
      }
    },
  },
  watch: {
    getLikeClass: {
      // 深度监听
      handler(val) {
        this.commentLike_id = val.technicalListDetail.is_zan; //取消点赞时用
        //进入详情页判断是否已经点赞
        this.likeClass = this.commentLike_id !== undefined && this.commentLike_id !== "";
        // this.$alert("commentLike_id "+this.commentLike_id)
        // this.$alert("likeClass "+this.likeClass)
        this.icon = val.icon;
        this.postLike = val.postLike;
        this.cancleLike = val.cancelLike;
      },
      deep: true

    }
  }
}
</script>

<style scoped lang="stylus">
.isZan-container {
  cursor: pointer
  font-size 14px
  color :grey
  .container-inner {
    position relative
    .isLike {  //点击后，图标选中样式
      color: #FF3300
    }
    .addOne { //动画+1
      position absolute
      right 0
      top: 0px
      font-size 20px
      font-weight bold
      color #FF3300
    }
    .cancleOne{ //-1的样式
      position absolute
      right 0
      top: 0
      font-size 20px
      font-weight bold
      color darkgreen
    }
    i {
      font-size 20px
    }

  }
  .container-inner:hover {
    /*color green*/
    font-size 16px
  }
}

</style>
