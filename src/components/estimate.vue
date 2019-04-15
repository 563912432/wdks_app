<template>
  <div class="estimate">
    <div class="head" @click="goBack">
      <i class="icon iconfont icon-fanhui1"></i>
    </div>
    <div class="top">
      <div class="title">能力评估</div>
      <!--img class="bg" src="/static/img/course_card.jpg" alt=""-->
    </div>
    <div class="content">
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-green">
            <i class="iconfont">&#xe676;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.personalNum}}</div>
            <div class="estimate-label">答题数量</div>
          </div>
        </div>
      </div>
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-orange">
            <i class="iconfont">&#xe676;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.maxNum}}</div>
            <div class="estimate-label">全网最高</div>
          </div>
        </div>
      </div>
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-green">
            <i class="iconfont">&#xe609;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.personalAccuracy}}%</div>
            <div class="estimate-label">正确率</div>
          </div>
        </div>
      </div>
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-orange">
            <i class="iconfont">&#xe609;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.avgAccuracy}}%</div>
            <div class="estimate-label">全网平均</div>
          </div>
        </div>
      </div>
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-green">
            <i class="iconfont">&#xe62e;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.signDays}}</div>
            <div class="estimate-label">签到天数</div>
          </div>
        </div>
      </div>
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-orange">
            <i class="iconfont">&#xe619;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.level}}</div>
            <div class="estimate-label">经验等级</div>
          </div>
        </div>
      </div>
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-green">
            <i class="iconfont">&#xe636;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.logintimes}}</div>
            <div class="estimate-label">登陆次数</div>
          </div>
        </div>
      </div>
      <div class="estimate-container">
        <div class="estimate-wrap">
          <div class="estimate-icon c-orange">
            <i class="iconfont">&#xe659;</i>
          </div>
          <div class="estimate-content">
            <div class="estimate-value">{{data.studyTime}}</div>
            <div class="estimate-label">学习时长</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="es6">
  import {COMMON} from '../mixins'
  import {Indicator, Toast, Loadmore} from 'mint-ui'
  import {mapGetters} from 'vuex'
  export default {
    mixins: [COMMON],
    name: 'estimate',
    data () {
      return {
        data: {
          personalNum: '0',
          personalAccuracy: '0',
          maxNum: '0',
          avgAccuracy: '0',
          signDays: '0',
          level: '',
          logintimes: '0',
          studyTime: '0',
        }
      }
    },
    created () {
      this.getData()
    },
    methods: {
      getData () {
        let that = this
        Indicator.open('载入中…')
        let uid = this.$store.state.userInfo.id
        this.$http.get(this.$store.state.host + 'Api/User/statistics/id/' + uid, {timeout: 10000}).then(response => {
          Indicator.close()
          if (response.ok && response.body.status === 1) {
            that.data = JSON.parse(response.body.info)
          } else {
            let instance = Toast({
              message: response.body.info,
              iconClass: 'mint-toast-icon mintui mintui-field-warning'
            })
            setTimeout(() => {
              instance.close()
            }, 1500)
          }
        }).catch(response => {
          let instance = Toast({
            message: '连接超时',
            iconClass: 'mint-toast-icon mintui mintui-field-warning'
          })
          setTimeout(() => {
            instance.close()
          }, 1500)
        })
      }
    },
    computed: {
      ...mapGetters(['uploadPath'])
    }
  }
</script>

<style type="text/css">
  .c-green{color: #00ce3c;}
  .c-orange{color: #ffb300;}
  .estimate {
    flex: 1;
    display: flex;
    width: 100%;
    height: 100%;
    flex-direction: column;
    overflow: hidden;
  }
  .estimate .head {
    position: absolute;
    z-index: 10;
    display: flex;
    justify-content: center;
    align-items: center;
    top: 3vw;
    left: 3vw;
    width: 30px;
    height: 30px;
    border: 1px solid #ddd;
    border-radius: 15px;
    background: rgba(7, 17, 27, 0.6);
    color: #fff;
  }
  .estimate .head .icon{
    font-size: 15px;
  }
  .estimate .top{
    height: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    color: #eeeeee;
    position: relative;
    overflow: hidden;
    background: #35495e;
  }
  .estimate .top .bg{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    filter: blur(1px);
    z-index: -1;
  }
  .estimate .content{
    flex: 1;
    display: flex;
    flex-flow: row wrap;
    justify-content: space-between;
    align-content: flex-start;
    overflow: auto;
    padding: 5px;
    padding-top: 10px;
  }
  .estimate-container{
    flex: 0 0 50%;
  }
  .estimate-wrap{
    margin: 5px;
    padding:30px 10px;
    background-color: #f8f7f7;
    border-radius: 5px;
    display: flex;
    flex-flow: row;
    align-items: center;
  }
  .estimate-icon{
    text-align: center;
    flex: 0 0 45%;
  }
  .estimate-icon .iconfont{font-size: 40px;}
  .estimate-content{
    flex: 0 0 55%;
  }
  .estimate-value{font-size: 16px;padding-bottom: 5px;}
  .estimate-label{font-size:14px;color:#999;}
</style>
