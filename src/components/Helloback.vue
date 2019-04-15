<template>
  <div class="hello">
    <div class="swiper" id="swiper">
      <mt-swipe :auto="400000">
        <mt-swipe-item v-for="item in gallery">
          <a :href="item.href"><div :style="'height:100%;background: url(\'' + item.src + '\') no-repeat center;background-size:auto 200px;'"></div></a>
        </mt-swipe-item>
      </mt-swipe>
    </div>
    <div class="wrapper">
      <!--div class="recommend bg-1">
        <div class="header">
          <div class="title">视频课</div>
        </div>
        <div class="list">
          <div v-for="item in videoCourse" class="item" @click="goDetail(item.course_type,item.id)">
            <div class="thumb">
              <img :src="uploadPath + item.thumb" alt="">
            </div>
            <div class="info">
              <div class="course-title">{{item.title}}</div>
              <div class="course-summary">{{item.brief}}</div>
            </div>
          </div>
        </div>
      </div-->
      <!--文章推荐-->
      <div class="recommend bg-1">
        <div class="header">
          <div class="title">
            <span class="title_tag"></span>
            <!--i class="iconfont icon-remind_fill"></i--> <span>热门咨询</span></div>
          <router-link tag="div" class="more clearfix" :to="{ path: 'news'}">
            更多 >
          </router-link>
        </div>
        <div class="list">
          <div v-for="item in news" class="item2" @click="goNews(item.id)">
            <div class="thumb">
              <img :src="uploadPath + item.thumb" alt="">
            </div>
            <div class="info">
              <div class="m_article_title">
                <span>{{item.title}}</span>
              </div>
              <div class="m_article_desc ">
                <div class="m_article_desc_l">
                  <span class="m_article_channel">{{item.source}}</span>
                  <span class="m_article_time">  {{item.addtime}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!--div class="section-title">— 学习推荐 —</div-->
      <!--视频课-->
      <div class="recommend bg-2">
        <div class="header">
          <div class="title"><span class="title_tag"></span><!--i class="iconfont icon-playon_fill"></i--><span>视频推荐</span></div>
          <!--div class="title">— 推荐视频 —</div-->
        </div>
        <div class="list">
          <div v-for="item in videoCourse" class="item2" @click="goDetail(item.course_type,item.id)">
            <div class="thumb">
              <img :src="uploadPath + item.thumb" alt="">
            </div>
            <div class="info">
              <div class="course-title">{{item.title}}</div>
              <div class="course-summary">{{item.brief}}</div>
              <!--<div class="course-price">￥{{item.promotion_price}}</div>-->
            </div>
          </div>
        </div>
      </div>
      <!--实操-->
      <div class="recommend bg-1">
        <div class="header">
          <div class="title"><span class="title_tag"></span><!--i class="iconfont icon-manage_fill"></i--><span>实训推荐</span></div>
          <!--div class="title">— 推荐实训 —</div-->
        </div>
        <div class="list">
          <div v-for="item in scCourse" class="item2" @click="goDetail(item.course_type,item.id)">
            <div class="thumb">
              <img :src="uploadPath + item.thumb" alt="">
            </div>
            <div class="info">
              <div class="course-title">{{item.title}}</div>
              <div class="course-summary">{{item.brief}}</div>
              <!--<div class="course-price">￥{{item.promotion_price}}</div>-->
            </div>
          </div>
        </div>
      </div>
      <!--题库-->
      <div class="recommend bg-2">
        <div class="header">
          <div class="title"><span class="title_tag"></span><!--i class="iconfont icon-zhuantilianxi"></i--><span>题库推荐</span></div>
          <!--div class="title">— 推荐题库 —</div-->
        </div>
        <div class="list">
          <div v-for="item in tkCourse" class="item2" @click="goDetail(item.course_type,item.id)">
            <div class="thumb">
              <img :src="uploadPath + item.thumb" alt="">
            </div>
            <div class="info">
              <div class="course-title">{{item.title}}</div>
              <div class="course-summary">{{item.brief}}</div>
              <!--<div class="course-price">￥{{item.promotion_price}}</div>-->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='stylus' rel='stylesheet/stylus'>
  .bg-1
    background-color #fff
  .bg-2
    background-color #fff
  .hello
    flex 1
    display flex
    flex-direction column
    overflow-x hidden
    overflow-y auto
    background #f1f1f1
  .section-title
    position relative
    text-align center
    padding 20px
    .more
      position absolute
      top 25px
      font-size 12px
      color #999
      right 20px
  .swiper
    flex 0 0 200px
    overflow auto
  .wrapper
    flex 1
    flex-direction column
  .recommend
    flex 1
    display flex
    flex-direction column
    margin 15px 0
    overflow auto
    .header
      position relative
      background:transparent
      .title
        text-align left
        padding 15px 0
        font-weight bold
        .title_tag
          display inline-block
          width 10px
          height 18px
          background #ff9900
          vertical-align text-top
          margin-right 10px
      .more
        position absolute
        top 15px
        font-size 16px
        color #999
        padding 5px 10px 5px 10px
        right 0
    .list
      flex 1
      display flex
      flex-flow row wrap
      align-content flex-start
      overflow auto
      padding 5px 20px
      .item
        flex 0 0 50%
      .item2
        width 100%
        height 100px
        display flex
        flex-direction row
        padding 5px
      .thumb
        width 120px
        min-width 120px
        padding-right 15px
        overflow hidden
        img
          height 80px
          border-radius 5px
      .info
        flex 1
        display flex
        flex-direction column
        overflow hidden
        .course-title
          height 30px;
          overflow hidden
          text-overflow ellipsis
          white-space nowrap
        .course-summary
          height 60px;
          font-size 12px
          line-height 20px
          color #999
          overflow hidden
          display -webkit-box;
          -webkit-box-orient vertical;
          -webkit-line-clamp 3;
        .course-price
          flex 1
          display flex
          align-items flex-end
          color #ff0000
          font-size 3.5vw
      .m_article_title
        font-size 16px
        margin-bottom .2rem
        color #404040
        line-height 24px
        width 100%
        display -webkit-box
        -webkit-line-clamp 2
        overflow hidden
        -webkit-line-break auto
        -webkit-box-orient vertical
      .m_article_channel
        display inline-block
        color #f33
        font-size 12px
        padding 2px
        line-height 12px
        margin-right .1rem
        border 1px solid #f33
        border-radius 1px
        background-size 100% 100%
      .m_article_time
        color #888
        display inline-block
        font-size 14px
        float right
</style>
<script type="es6">
  import 'mint-ui/lib/style.css'
  import {Swipe} from 'mint-ui/lib/swipe'
  import {SwipeItem} from 'mint-ui/lib/swipe-item'
  import {Loadmore} from 'mint-ui/lib/loadmore'
  import {Toast, Indicator} from 'mint-ui'
  import {mapGetters} from 'vuex'
  export default {
    name: 'hello',
    components: {
      Swipe,
      SwipeItem,
      Loadmore
    },
    data () {
      return {
        allLoaded: false,
        topStatus: '',
        scCourse: [],
        videoCourse: [],
        tkCourse: [],
        news: [],
        gallery: []
      }
    },
    created () {
      this.$store.commit('setSelected', '1')
      this.getData()
    },
    methods: {
      loadBottom () {
        let that = this
        setTimeout(function () {
          that.$refs.loadmore.onBottomLoaded()
        }, 1500)
        this.allLoaded = true
      },
      handleBottomChange (status) {
        this.topStatus = status
      },
      goRouter (index) {
        this.$router.push({path: '/tk/' + index})
      },
      getData () {
        Indicator.open({
          text: '数据获取中...',
          spinnerType: 'snake'
        })
        let that = this
        let callBack = {
          success: function (data) {
            that.tkCourse = data.tkCourse,
            that.videoCourse = data.videoCourse,
            that.scCourse = data.scCourse,
            that.gallery = data.gallery,
            that.news = data.news,
            Indicator.close()
          },
          error: function (error) {
            if (!error) {
              error = '数据获取失败'
            }
            Indicator.close()
            Toast({
              message: error,
              iconClass: 'mint-toast-icon mintui mintui-field-warning',
              duration: 1500
            })
          }
        }
        this.$store.dispatch('getIndexData', callBack)
      },
      goDetail (type, id) {
        switch (+type) {
          case this.$store.state.courseType.tk:
          case this.$store.state.courseType.op:
            this.$router.push({path: '/tkDetail/' + id})
            break
          case this.$store.state.courseType.video:
            this.$router.push({path: '/video/' + id})
            break
        }
      },
      goNews (id) {
        this.$router.push({path: '/news/' + id})
      }
    },
    computed: {
      ...mapGetters(['uploadPath'])
    }
  }
</script>
