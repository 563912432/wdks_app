<template>
  <div class="notes">
    <div class="head" @click="goBack">
      <i class="icon iconfont icon-fanhui1"></i>
    </div>
    <div class="top">
      <div class="title">我的笔记</div>
      <!--img class="bg" src="/static/img/course_card.jpg" alt=""-->
    </div>
    <div class="head-control-tools">
      <div class="search-item" @click="selectCidVisable = true">
        {{curentCourseTitle}}
      </div>
      <div class="search-item" @click="openNoteDatePicker">
        {{curentDate}}
      </div>
    </div>
    <div class="chapter" v-if="notes && notes.length > 0">
      <mt-loadmore :top-method="loadTop" :bottom-method="loadBottom" :bottomPullText='bottomText' :bottom-all-loaded="allLoaded" :auto-fill="false" ref="loadmore">
        <div class="list">
          <div v-for="item in notes" class="note-item">
            <div class="list-title">
              <span class="list-title-text">来源: {{item.course.title}}</span>
              <span class="list-title-time">{{item.created_at}}</span>
            </div>
            <div class="list-content">
              {{item.note}}
            </div>
            <div class="list-tk">
              <noteTk :exam="item.tk"></noteTk>
            </div>
          </div>
        </div>
      </mt-loadmore>
    </div>
    <div class="chapter" v-else>
      <div class="empty">
        <i class="icon iconfont icon-meiyougengduo" style="font-size: 40vw; color: rgba(7,17,27,0.1)"></i>
        <small>没有找到笔记</small>
      </div>
    </div>
    <mt-popup v-model="selectCidVisable" position="bottom" class="mint-popup">
      <mt-picker :slots="this.coursesData" :visible-item-count="5" :show-toolbar="true"  ref="picker" value-key="title">
        <mt-button @click="selectCid" type="primary" class="sure">确认</mt-button>
      </mt-picker>
    </mt-popup>
    <mt-datetime-picker
      ref="datepicker"
      v-model="cur_date"
      type="date"
      @confirm="selectNoteDate"
      year-format="{value} 年"
      month-format="{value} 月"
      date-format="{value} 日">
    </mt-datetime-picker>
  </div>
</template>

<script type="es6">
  import {COMMON} from '../mixins'
  import {Indicator, Toast, Loadmore, Badge, Picker, Popup} from 'mint-ui'
  import noteTk from '@/components/noteTk'
  import {mapGetters} from 'vuex'
  export default {
    mixins: [COMMON],
    name: 'noteList',
    components: {
      noteTk,
      Badge,
      Picker,
      Popup
    },
    data () {
      return {
        notes: [],
        p: 1,
        pageTotal: '',
        bottomText: '上拉加载更多...',
        allLoaded: false,
        selectCidVisable: false,
        courses: [],
        cur_course: null,
        cur_date: null
      }
    },
    created () {
      this.$store.commit('setSelected', '1')
      this.getNoteList(true)
    },
    methods: {
      openNoteDatePicker () {
        this.$refs.datepicker.open()
      },
      selectNoteDate () {
        this.p = 1
        this.notes = []
        this.getNoteList()
      },
      selectCid () {
        this.p = 1
        this.cur_course = this.$refs.picker.getValues()[0]['id']
        this.notes = []
        this.getNoteList()
        this.selectCidVisable = false
      },
      //下拉刷新
      loadTop () {
        this.p = 1
        this.notes = []
        this.getNoteList()
        this.$refs.loadmore.onTopLoaded();
      },
      loadBottom () {
        this.p += 1
        this.getNoteList()
        this.$refs.loadmore.onBottomLoaded()
      },
      getNoteList (getSearchItems) {
        let that = this
        let data = {
          p: that.p,
          uid: that.$store.state.userInfo.id,
          cid: that.cur_course,
          noteDate: that.cur_date ? that.dateToString(that.cur_date) : '',
          searchOption: getSearchItems,
          success: function (data) {
            //alert('qunimei')
            if (getSearchItems) {
              that.courses = data.courses
            }
            that.notes = that.notes.concat(data.list)
            that.pageTotal = data.pageTotal
            that.isLastPage()
          },
          error: function (error) {

          }
        }
        this.$store.dispatch('getNoteList', data)
      },
      isLastPage () {
        if(this.p == this.pageTotal){
          this.allLoaded = true;
        }else{
          this.allLoaded = false;
        }
      },
      dateToString (date) {
        let year = date.getFullYear()
        let month = (date.getMonth() + 1).toString()
        let day = (date.getDate()).toString()
        if (month.length === 1) {
          month = '0' + month
        }
        if (day.length === 1) {
          day = '0' + day
        }
        return year + '-' + month + '-' + day
      }
    },
    computed: {
      ...mapGetters(['uploadPath']),
      coursesData () {
        return [
          {
            flex: 1,
            values: this.courses,
            className: 'slot1',
            textAlign: 'center'
          }
        ]
      },
      curentCourseTitle () {
        for (let i=0; i<this.courses.length; i++){
          if (this.courses[i]['id'] == this.cur_course) {
            return this.courses[i]['title']
          }
        }
        return '按科目筛选'
      },
      curentDate () {
        return this.cur_date ? this.dateToString(this.cur_date) : '按日期'
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .notes
    flex 1
    display flex
    width 100%
    height 100%
    flex-direction column
    overflow hidden
    background-color #f8f8f8
    .empty
      display flex
      flex 1
      height 100%
      flex-direction column
      justify-content center
      align-items center
      small
        color #999
    .head
      position absolute
      z-index 10
      display flex
      justify-content center
      align-items center
      top 3vw
      left 3vw
      width 30px
      height 30px
      border 1px solid #ddd
      border-radius 15px
      background rgba(7, 17, 27, 0.6)
      color #fff
      .icon
        font-size 15px
    .top
      height 50px
      display flex
      align-items center
      justify-content center
      font-size 18px
      color #eeeeee
      position relative
      overflow hidden
      background #35495e
      .bg
        position absolute
        top 0
        left 0
        width 100%
        height 100%
        filter blur(1px)
        z-index -1
    .chapter
      flex 1
      overflow auto
      .list
        overflow hidden
        .note-item
          width 100%
          padding 0 15px
          margin 10px 0
          overflow hidden
          background-color #fff
          .list-title
            display flex
            flex-flow row
            color #999
            width 100%
            padding 10px 0
            font-size 14px
            line-height 14px
            justify-content space-between
            .list-title-text
              flex 1
              overflow hidden
              text-overflow ellipsis
              white-space nowrap
            .list-title-time
              text-align right
              flex 0 0 80px
          .list-content
            background-color #f1f1f1
            padding 10px
            width 100%
            font-size 15px
          .list-tk
            padding 5px 0
            color #999
            width 100%
  .head-control-tools
    margin-top 1px
    height 30px
    display flex
    flex-direction row
    justify-content center
    align-items center
    box-sizing border-box
    background-color #fff
    color #333
    font-size 14px
    .search-item
      height 100%
      flex 1
      display flex
      justify-content center
      align-items center
      .icon
        font-size 14px
        margin-right 2px
        color #0086b3
  .mint-popup
    width 100%
    .sure
      width 100%
</style>
