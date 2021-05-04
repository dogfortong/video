<template>
  <div>
    <div>
      <the-header />
    </div>
    <div class="player">
      <div class="left">
        <h1>{{title}}</h1>
        <p>{{time}}</p>
        <VueMiniPlayer :video="video" />
        <comment v-if="author.id !== '-1'" @doSend="doSend($event)" @doChildSend="doChildSend(arguments)"
          :commentList="commentList" :commentNum="commentNum" :label="label" :avatar="avatar" :placeholder="placeholder"
          :minRows="minRows" :maxRows="maxRows" :authorId="parseInt(author.id)"></comment>
      </div>
      <div class="right">
        <information :author-name="author.nickName" :author-avatar="author.avatar"
          :introduction="introduction" :give-follow-like="giveFollowLike" :give-follow-collection="giveFollowCollection"
          @getArticleDetail="getArticleDetail"></information>
      </div>


    </div>
  </div>
</template>

<script>
  import comment from '../components/Comment.vue'
  import Information from "../components/Information.vue";
  import TheHeader from '../components/TheHeader.vue'
  export default {
    name: 'player',

    components: {
      Information,
      comment,
      TheHeader,
    },

    data() {
      return {
        // 视频部分
        vid: String,
        author: {
          id: '-1',
          nickName: '',
          avatar: ''
        },
        title: "IU Blueming MV",
        time: `2021.4.18 12:07`,
        introduction: '',
        video: {
          url: "",
          cover: ""
        },
        likeNum: 0,
        collectionNum: 0,

        giveFollowLike: {
          technicalListDetail: { // 在进入页面时请求数据获取
            t_zan: 12,
            is_zan: true
          },
          icon: "",
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

        // 评论部分
        uid: String,
        avatar: '',

        label: "作者",
        placeholder: "说点什么吧",
        minRows: 4,
        maxRows: 4,
        commentNum: 0,
        commentList: [{
            id: 1,
            commentUser: {
              id: 3,
              nickName: 'User1',
              avatar: require('../../static/image/a1.jpg')
            },
            content: "评论1",
            createDate: '2019-9-23 17:36:02',
          },
          {
            id: 2,
            commentUser: {
              id: 1,
              nickName: 'User2',
              avatar: require('../../static/image/a2.jpg')
            },
            content: "评论2",
            createDate: '2019-9-23 17:36:02',
          }

        ]
      };
    },
    watch: {},
    computed: {},
    filters: {},
    methods: {
      getArticleDetail: function() {
        this.getVideoInfo()
      },

      makeComment(uid, vid, rcid, content) {
        this.$http.post('api/make/comment', {
          uid: uid,
          vid: vid,
          rcid: rcid,
          content: content
        }, {
          emulateJSON: true
        }).then((response) => {
          this.updated()
        }, (response) => {
          this.$alert('评论失败', {
            dangerouslyUseHTMLString: true
          })
        });
      },

      doSend(content) {
        //this.$alert(this.vid+"    "+this.uid+"    "+content)
        this.makeComment(this.uid, this.vid, null, content)
        this.getCommentInfo()
      },
      doChildSend(args) {
        this.$alert("评论区发送事件：" + args[0] + " " + args[1] + " " + args[2])
        this.makeComment(this.uid, this.vid, args[2], args[0])
        this.getCommentInfo()
      },

      getVideoInfo() {
        // 获取视频信息
        this.$http.post('api/video/' + this.vid + '/' + this.uid).then((response) => {
          console.log(response);
          this.author = response.data.author
          this.title = response.data.title
          this.time = response.data.time
          this.introduction = response.data.introduction
          this.video.url = response.data.videoUrl
          this.video.cover = response.data.coverUrl
          this.commentNum = response.data.comment
          this.giveFollowLike.technicalListDetail.t_zan = response.data.like
          this.giveFollowLike.technicalListDetail.is_zan = response.data.likeId
          this.giveFollowLike.postLike = 'api/postLike/' + this.vid + '/' + this.uid
          this.giveFollowLike.cancelLike = 'api/cancelLike/' + this.vid + '/'
          this.giveFollowCollection.technicalListDetail.t_zan = response.data.collection
          this.giveFollowCollection.technicalListDetail.is_zan = response.data.collectionId
          this.giveFollowCollection.postLike = 'api/postCollection/' + this.vid + '/' + this.uid
          this.giveFollowCollection.cancelLike = 'api/cancelCollection/' + this.vid + '/'
        }, (response) => {
          this.$alert('获取视频失败', {
            dangerouslyUseHTMLString: true
          })
        });

      },
      getMyInfo() {
        this.$http.post('api/user/' + this.uid).then((response) => {
          this.avatar = response.data.avatar
        }, (response) => {
          this.$alert('获取自己的头像失败', {
            dangerouslyUseHTMLString: true
          })
        });
      },
      getCommentInfo() {
        // 获取评论
        this.$http.post('api/comment/' + this.vid).then((response) => {
          this.commentList = response.data
        }, (response) => {
          this.$alert('获取评论失败', {
            dangerouslyUseHTMLString: true
          })
        });
      }


    },
    created() {
      this.vid = this.$route.query.vid
      this.uid = this.$route.query.uid
      this.getMyInfo()
      //this.$alert(this.$route.query.vid + this.$route.query.uid, {dangerouslyUseHTMLString:true})
    },

    mounted() {
      this.getVideoInfo()
      this.getCommentInfo()
    },
    updated() {
      // this.getVideoInfo()
      // this.getCommentInfo()
    },
    beforeDestroy() {},
    destroyed() {}
  };
</script>

<style scoped>
  h1 {
    display: block;
    text-align: left;
    margin: 0 auto;
  }

  p {
    display: block;
    color: darkgrey;
    text-align: left;
    margin: 0 auto;
  }

  .player {
    display: inline-block;
    width: 1300px;
    height: 1200px;
    /* margin: 0 auto; */
    margin-left: 420px;
  }

  .left {
    display: inline-block;
    /* vertical-align: top; */
    width: 70%;
    /* margin: 0 auto; */
  }

  .right {
    display: inline-block;
    float: right;
    /* vertical-align: top; */
    width: 25%;
    margin-top: 50px;
    margin-left: 30px;
  }
</style>
