<template>
  <div class="noteTk">
    <mt-button @click="toggle" size="small">{{title}}</mt-button>
    <div class="list" v-show="showWhole">
      <div class="list-for">
        <div class="even" v-if="+exam.type === typeList.single ||
                                                        +exam.type === typeList.single_five ||
                                                        +exam.type === typeList.zuhexuanze ||
                                                        +exam.type === typeList.qhzhdx ||
                                                        +exam.type === typeList.qhzhmx ||
                                                        +exam.type === typeList.multi ||
                                                        +exam.type === typeList.multi_five">
          <div class="row">
            <mt-badge size="small" type="success">
              {{ exam.typename }}
            </mt-badge>
            <span v-html="exam.title"></span></div>
          <div class="row">
            <p v-for="(opt, index) in exam.option"><span style="color: #66ccff">{{az[index]}}</span>、{{opt}}</p>
          </div>
          <div class="row">
            <mt-badge size="small" color="#57bacd">
              正确答案
            </mt-badge>
            {{exam.right_answer.toString()}}
          </div>
          <div class="row">
            <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
            <span v-html="exam.parse ? exam.parse : '暂无解析'"></span>
          </div>
        </div>
        <!--判断题-->
        <div class="item" v-if="+exam.type === typeList.judge">
          <div class="row">
            <mt-badge size="small" type="success">
              {{ exam.typename }}
            </mt-badge>
            <span v-html="exam.title"></span>
          </div>
          <div class="row">
            <mt-badge size="small" color="#57bacd">正确答案</mt-badge>
            {{exam.right_answer === 'A' ? '正确' : '错误'}}
          </div>
          <div class="row">
            <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
            <span v-html="exam.parse ? exam.parse : '暂无解析'"></span>
          </div>
        </div>
        <!--不定向选择题-->
        <div class="item" v-if="+exam.type === typeList.material">
          <div class="exam-title">
            <mt-badge size="small" type="success">
              {{ exam.typename }}
            </mt-badge>
            <span v-html="exam.material"></span>
          </div>
          <div class="exam-title" v-for="(title, title_index) in exam.title">
            <div v-html="title"></div>
            <div style="margin: 5px 10px">
              <div v-for="(opt, opt_index) in exam.option[title_index]">
                <span style="color: #66ccff">{{az[opt_index]}}</span>、{{opt}}
              </div>
              <div class="row">
                <mt-badge size="small" color="#57bacd">正确答案</mt-badge>
                {{exam.right_answer[title_index].toString()}}
              </div>
              <div class="row">
                <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
                <span v-html="exam.parse[title_index]"></span>
              </div>
            </div>
          </div>
        </div>
        <!--中级：材料分析题-->
        <div class="item" v-if="+exam.type === typeList.jisuan_fenxi">
          <div class="row">
            <mt-badge size="small" type="success">{{ exam.typename }}
            </mt-badge>
            <span v-html="exam.material"></span></div>
          <div class="row" v-for="(title, title_index) in exam.title">
            <span v-html="title_index+1 + '、' + title"></span>
            <div class="row">
              <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
              <span v-html="exam.parse[title_index]"></span>
            </div>
          </div>
          <div v-if="exam.video" class="row">
            <mt-badge size="small" type="primary">
              <span @click="showVideo(exam.video)">视频解析</span>
            </mt-badge>
          </div>
        </div>
        <!--配伍题-->
        <div class="item" v-if="+exam.type === typeList.pw">
          <mt-badge size="small" type="success">
            {{ exam.typename }}
          </mt-badge>
          <div style="margin: 5px 10px">
            <mt-badge size="small" color="#57bacd">公共选项</mt-badge>
            <div class="row" v-for="(opt, opt_index) in exam.option">
              <span style="color: #66ccff">{{az[opt_index]}}</span>、{{opt}}
            </div>
          </div>
          <div class="row" v-for="(title, title_index) in exam.title">
            <span v-html="(title_index+1) + '、' + title"></span>
            <div style="margin: 5px 10px">
              <div class="row">
                <mt-badge size="small" color="#57bacd">正确答案</mt-badge>
                {{exam.right_answer[title_index].toString()}}
              </div>
              <div class="row">
                <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
                <span v-html="exam.parse"></span>
              </div>
            </div>
          </div>
        </div>
        <!--中级简答题-->
        <!--中级：材料分析题 建筑案例 活动设计 教学设计 材料分析-->
        <div class="item" v-if="+exam.type === typeList.zjjd ||
                                +exam.type === typeList.jzanli ||
                                +exam.type === typeList.zh ||
                                +exam.type === typeList.huodongsheji ||
                                +exam.type === typeList.jiaoxuesheji ||
                                +exam.type === typeList.cailiaofenxi">
          <div class="row">
            <mt-badge size="small" type="success">{{ exam.typename }}
            </mt-badge>
            <span v-html="exam.material"></span>
          </div>
          <div class="row" v-for="(title, title_index) in exam.title">
            <span v-html="title_index+1 + '、' + title"></span>
            <div class="row">
              <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
              <span v-html="exam.parse[title_index]"></span>
            </div>
          </div>
        </div>
        <!--辨析题-->
        <div class="item" v-if="+exam.type === typeList.bianxi ||
                                +exam.type === typeList.jianda ||
                                +exam.type === typeList.xiezuo ||
                                +exam.type === typeList.lunshu">
          <div class="row">
            <mt-badge size="small" type="success">{{ exam.typename }}
            </mt-badge>
            <span v-html="exam.title"></span>
          </div>
          <div class="row">
            <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
            <span v-html="exam.parse"></span>
          </div>
        </div>
        <!--综合分析选择题-->
        <!--不定向选择题-->
        <div class="item" v-if="+exam.type === typeList.zhfxxz">
          <div class="exam-title">
            <mt-badge size="small" type="success">{{ exam.typename }}
            </mt-badge>
            <span v-html="exam.material"></span>
          </div>
          <div class="exam-title" v-for="(title, title_index) in exam.title">
            ({{title_index+1}})、
            <span v-html="title"></span>
            <div class="row">
              <div v-for="(opt, opt_index) in exam.option[title_index]">
                <span style="color: #66ccff">{{az[opt_index]}}</span>、{{opt}}
              </div>
              <div class="row">
                <mt-badge size="small" color="#57bacd">正确答案</mt-badge>
                {exam.right_answer[title_index].toString()}}
              </div>
              <div class="row">
                <mt-badge size="small" color="#57bacd">名师解析</mt-badge>
                <span v-html="exam.parse[title_index]"></span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="es6">
  import {COMMON} from '../mixins'
  import {Toast, Indicator, Button} from 'mint-ui'
  import {mapState, mapGetters} from 'vuex'
  export default {
    name: 'noteTk',
    props: {
      exam: {
        type: Object
      }
    },
    computed: {
      ...mapState(['typeList']),
      ...mapGetters(['uploadPath'])
    },
    components: {
      Button
    },
    data () {
      return {
        az: ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'],
        dx: ['一', '二', '三', '四', '五', '六', '七', '八'],
        examType: {
          single: 1,
          multi: 5,
          judge: 8,
          material: 15
        },
        odd: 'item odd',
        even: 'item even',
        showWhole: false,
        title: '展开题目'
      }
    },
    methods: {
      toggle () {
        if (this.showWhole) {
          this.showWhole = false
          this.title = '展开题目'
        } else {
          this.showWhole = true
          this.title = '隐藏题目'
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .noteTk
    flex 1
    position relative
    z-index 10
    left 0
    width 100%
    height 100%
    overflow hidden
    display flex
    flex-direction column
    .list
      padding 0 5px 0
      font-size 14px
      flex 1
      overflow auto
      .item
        padding 10px 5px
        border-bottom 1px dashed #f3f3f3
        .row
          padding 2px 0
      .odd
        background #FBF9FE
        .exam-option
          padding 5px 10px
          display flex
          flex-direction column
  .waiter-button{
    position: fixed;
    right: 10px;
    bottom: 10%;
    z-index: 100;
    // background: rgba(255,255,255,0.8);
    background-color: #fff;
    border-radius: 25px;
  }
  .waiter-mask{
    opacity: 0.5;
    background: #000;
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    z-index: 2004;
  }
  .waiter-wrap{
    z-index: 2010;
    position: fixed;
    top: 50%;
    left: 50%;
    -webkit-transform: translate3d(-50%, -50%, 0);
    transform: translate3d(-50%, -50%, 0);
    background-color: #fff;
    width: 85%;
    padding: 10px;
    border-radius: 3px;
    font-size: 16px;
    -webkit-user-select: none;
    overflow: hidden;
    -webkit-backface-visibility: hidden;
    backface-visibility: hidden;
    -webkit-transition: .2s;
    transition: .2s;
  }
  .waiter-wrap img{
    width: 100%;
  }
</style>
