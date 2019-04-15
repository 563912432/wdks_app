<template>
  <div class="header">
    <div class="top">
      <div class="avatar">
        <img :src="uploadPath + userInfo.headimg" width="80" height="80">
      </div>
      <div class="info">
        <span>{{userInfo.tel}}   <i class="icon iconfont icon-vip"></i></span>
        <small>{{userInfo.wisdom}}</small>
      </div>
    </div>
    <div class="bg">
      <img :src="uploadPath + userInfo.headimg">
    </div>
    <div class="nav">
      <div class="nav-item" @click="goMyCourse">
        <i class="icon iconfont icon-manage_fill"></i>
        <span>我的课程</span>
      </div>
      <!--<div class="nav-item">-->
        <!--<i class="icon iconfont icon-huizhimobangaoqingtubiaosvgfuzhi" style="color: #87c38f"></i>-->
        <!--<span>笔记</span>-->
      <!--</div>-->
      <!--<div class="nav-item">-->
        <!--<i class="icon iconfont icon-fuwu-copy" style="color: #d4237a"></i>-->
        <!--<span>服务</span>-->
      <!--</div>-->
      <div class="nav-item" @click="goSpendLog">
        <i class="icon iconfont icon-coupons_fill"></i>
        <span>我的账单</span>
      </div>
      <div class="nav-item" @click="openReminderPicker">
        <i class="icon iconfont icon-remind_fill"></i>
        <div style="text-align: center">
          <span v-if="this.userInfo.reminder"><span style="color:#F37B1D">{{reminder.dayleft}}</span> 天<br /></span>
          <span>考试提醒</span>
        </div>
      </div>
    </div>
    <div class="msg-box" @click='goMsgCenter'>
      <div class="has-msg"></div>
      <i class="icon iconfont icon-xinxi"></i>
    </div>
    <mt-datetime-picker
      ref="picker"
      v-model="reminderDate"
      type="date"
      :startDate="startDate"
      @confirm="setReminder"
      year-format="{value} 年"
      month-format="{value} 月"
      date-format="{value} 日">
    </mt-datetime-picker>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import { DatetimePicker } from 'mint-ui'
  export default {
    name: 'memberHeader',
    components: {
      DatetimePicker
    },
    data () {
      return {
        reminderDate: null,
        startDate: new Date(new Date().getTime() + 86400000)
      }
    },
    created () {
      this.initData()
    },
    computed: {
      ...mapGetters(['uploadPath', 'createSignature']),
      reminder () {
        if (this.$store.state.userInfo.reminder) {
          let reminder = JSON.parse(this.$store.state.userInfo.reminder)[0]
          reminder.dayleft = this.dayLeft(reminder.time)
          return reminder
        }
        return null
      }
    },
    props: {
      userInfo: {
        type: Object
      }
    },
    methods: {
      initData () {
        // 设置考试倒计时提醒的初始日期
        this.reminderDate = (this.reminder && this.reminder.dayleft > 0 ? new Date(this.reminder.time.replace('-', '/')) : this.startDate)
      },
      goMsgCenter () {
        this.$router.push({'path': '/message'})
      },
      goSpendLog () {
        this.$router.push({'path': '/spendlog'})
      },
      goMyCourse () {
        this.$router.push({'path': '/mycourse'})
      },
      openReminderPicker () {
        this.$refs.picker.open()
      },
      setReminder () {
        // 保存reminder
        let signatureObj = this.createSignature
        let data = {
          random: signatureObj.random,
          signature: signatureObj.signature,
          uid: this.$store.state.userInfo.id,
          reminder: [{
            time: this.dateToString(this.reminderDate)
          }]
        }
        this.$store.commit('setReminder', JSON.stringify(data.reminder))
        this.$http.post(this.$store.state.host + 'Api/User/serReminder', data, {
          timeout: 5000,
          emulateJSON: true
        }).then(response => {

        }).catch((response) => {

        })
      },
      dayLeft (sDate1) {
        sDate1 = Date.parse(sDate1)
        let sDate2 = Date.parse(new Date())
        let timeLeft = sDate1 - sDate2
        if (timeLeft > 0) {
          return Math.floor(timeLeft / (24 * 3600 * 1000))
        } else {
          return 0
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
    }
  }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  .header
    position relative
    background rgba(0,0,0,0.5)
    .top
      height 120px
      display flex
      flex-direction row
      align-items center
      overflow hidden
      .avatar
        margin 15px
        width 82px
        height 82px
        border-radius 41px
        border 1px solid #eee
        display flex
        justify-content center
        align-items center
        img
          border-radius 40px
          width 80px
          height 80px
      .info
        display flex
        flex-direction column
        color #fff
        flex 1
        padding-top 3px
        small
          font-size 12px
          color: #999
        span
          font-size 22px
          .icon-vip
            font-size 22px
            color orange
    .nav
      background #fff
      display flex
      flex-flow row wrap
      padding 10px
      border-bottom 1px solid #eee
      height 50px
      .nav-item
        flex 0 0 33%
        display flex
        flex-direction row
        justify-content center
        align-items center
        font-size 16px
        vertical-align baseline
        .icon
          color #4caf50
          vertical-align baseline
          font-size 18px
          margin-right 3px
      .nav-item:nth-child(1)
        border-right 1px solid #eee
    .bg
      position absolute
      width 100%
      height 120px
      top 0
      left 0
      z-index -1
      img
        width 100%
        height 100%
        filter blur(10px)
    .msg-box
      position absolute
      top 10px
      right 10px
      .icon
        font-size 25px
        color #999

</style>
