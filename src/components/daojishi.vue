<template>
  <div class="djs">
    剩余时间: {{ leftTime }} <!--span class="rest" @click="rest">暂停</span-->
  </div>
</template>

<script type="text/ecmascript-6">
  import {Toast, MessageBox} from 'mint-ui'
  export default {
    name: 'djs',
    props: {
      time: {
        type: Number,
        require: true
      }
    },
    components: {
      Toast,
      MessageBox
    },
    computed: {
     /* leftTime () {
        let that = this
        setInterval(function () {
          that.oTime--
          that.$store.commit('setExamLeftTime', that.oTime)
          that.text = that.transSeconds(that.oTime, that)
        }, 1000)
        return this.text
      } */
    },
    created () {
      this.calcLeftTime(this.$store.state.paperInfo ? this.$store.state.paperInfo.time : 0)
    },
    destroyed () {
      clearInterval(this.timer)
    },
    data () {
      return {
        text: '',
        leftTime: '00:00:00',
        timer: null
      }
    },
    methods: {
      calcLeftTime (oTime) {
        let that = this
        this.timer = setInterval(function () {
          oTime--
          that.$store.commit('setExamLeftTime', oTime)
          that.leftTime = that.transSeconds(oTime, that)
        }, 1000)
      },
      transSeconds (time, obj) {
        let day = 0
        let hour = 0
        let minute = 0
        let second = 0
        if (time > 0) {
          day = Math.floor(time / (60 * 60 * 24))
          hour = Math.floor(time / (60 * 60)) - (day * 24)
          minute = Math.floor(time / 60) - (day * 24 * 60) - (hour * 60)
          second = Math.floor(time - (day * 24 * 60 * 60) - (hour * 60 * 60) - (minute * 60))
        }
        if (time === 180) {
          Toast({
            message: '还有最后3分钟，加油哦',
            duration: 2500
          })
        }
        if (time === 0) {
          let instance = Toast({
            message: '时间到了，即将交卷……'
          })
          setTimeout(function () {
            instance.close()
            obj.$router.push({path: '/result/' + obj.$route.params.pid})
          }, 2500)
        }
        if (hour <= 9) hour = '0' + hour
        if (minute <= 9) minute = '0' + minute
        if (second <= 9) second = '0' + second
        return (hour + ' :' + minute + ' :' + second)
      },
      rest () {
        let that = this
        if (this.timer) {
          // 暂停倒计时
          clearInterval(this.timer)
          this.timer = null
          // 弹窗提示
          MessageBox({message: '考试暂停，休息一下！', confirmButtonText: '继续考试', closeOnClickModal: false}).then(action => {
            that.calcLeftTime(this.$store.state.examState.leftTime)
          })
        }
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .djs
    font-size 12px
    color #999
  .rest{
    padding-left: 10px;
    color: #eee;
  }
</style>
