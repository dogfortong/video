<template>
  <div>
    <avatar :avatar=authorAvatar class="avatar"></avatar>
    <div class="nickname">{{ authorName }}</div>

    <div class="intro">{{introduction}}</div>

    <is-like class= "like" :getLikeClass="giveFollowLike" @getArticleDetail="getArticleDetail"></is-like>
    <is-like class= "collection" :getLikeClass="giveFollowCollection" @getArticleDetail="getArticleDetail"></is-like>

  </div>
</template>

<script>
import avatar from '../components/Avatar.vue'
import IsLike from "../components/IsLike";
export default {
  name: "Information",

  props:{
    authorName: {
      type:String,
      default:"susns",
    },
    authorAvatar: {
      type: String,
      default: require('../../static/image/a1.jpg'),
    },

    introduction: {
      type:String,
      default:'还未加载出来视频介绍',
    },

    giveFollowLike: {
      technicalListDetail: { // 在进入页面时请求数据获取
        t_zan: 12,
        is_zan: String
      },
      icon: '',
      postLike: "",
      cancelLike: "",
      title: "点赞数"
    },
    giveFollowCollection: {
      technicalListDetail: { // 在进入页面时请求数据获取
        t_zan: 12,
        is_zan: String
      },
      icon: '',
      postLike: "",
      cancelLike: "",
      title: "收藏数"
    },

  },

  components:{
    IsLike,
    avatar,
  },

  data(){
    return {

    }

  },

  methods: {
    getArticleDetail:function (){
      this.$emit('getArticleDetail')
    }
  },
  created() {
    //this.$alert("create "+this.introduction)
  },
  watch:{
    // authorName:{
    //   handler(val){
    //     this.$alert(val)
    //   },
    //   deep:true
    // }
  }
}
</script>

<style scoped>
.intro,.like,.collection{
  display: block;
  margin-left: 10px;
}
.nickname{
  display: inline-block;
  vertical-align: top;
  font-size: 14px;
}
.avatar{
  display: inline-block;
  vertical-align: top;
}



</style>
