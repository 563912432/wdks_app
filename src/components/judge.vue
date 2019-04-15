<template>
  <div class="judge">
    <div class="title">
      <mt-badge size="small">题目</mt-badge>
      <span v-html="exam.title"></span>
    </div>
    <div class="choice">
      <mt-radio title="请选择" v-model="value" :options='options'></mt-radio>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import { Badge, Radio, Toast } from 'mint-ui'
  import {mapGetters} from 'vuex'
  export default {
    name: 'judge',
    props: {
      exam: {
        type: Object,
        require: true
      }
    },
    computed: {
      ...mapGetters(['createSignature'])
    },
    components: {
      Badge,
      Radio,
      Toast
    },
    data () {
      return {
        options: [
          {label: '对', value: 'A'},
          {label: '错', value: 'B'}
        ],
        value: ''
      }
    },
    mounted () {
      // if (this.$route.params.progress_id) {
      //   this.watchExam()
      // }
      this.watchExam()
    },
    methods: {
      watchExam () {
        let data = this.$store.state.examState.userAnswer
        let bool = false
        for (let i = 0; i < data.length; i++) {
          if (data[i].no === this.exam.number) {
            this.value = data[i].val
            bool = true
          }
        }
        if (!bool) {
          this.value = ''
        }
      },
      answer (data) {
        this.$http.post(this.$store.state.host + 'Api/Exam/answer', data, {
          timeout: 5000,
          emulateJSON: true
        }).then(response => {

        }).catch((response) => {

        })
      }
    },
    watch: {
      exam () {
        this.watchExam()
      },
      value (newValue) {
        if (newValue) {
          if (this.$route.name === 'chapter_exam' || this.$route.name === 'error_exam') {
            let that = this
            // 验证是否答对了
            if (this.value === this.exam.right_answer) {
              let instance = Toast({
                message: '恭喜，答对了',
                iconClass: 'mint-toast-icon mintui mintui-success'
              })
              setTimeout(function () {
                instance.close()
              }, 800)
            } else {
              that.$store.commit('setExamParse', true)
              if (this.$route.name === 'chapter_exam') {
                // 记录错题
                let data = {
                  param: {
                    cid: this.$route.params.tid,
                    uid: this.$store.state.userInfo.id,
                    error: this.exam.number
                  }
                }
                this.$store.dispatch('saveError', data)
              }
            }
          }
          let data = this.$store.state.examState.userAnswer
          let change = false
          for (let i = 0; i < data.length; i++) {
            if (data[i].no === this.exam.number) {
              data[i].val = newValue
              change = true
            }
          }
          if (change === false) {
            data.push({no: this.exam.number, val: newValue})
          }
          this.$store.commit('saveAnswer', data)
          let signatureObj = this.createSignature
          if (this.$route.name === 'chapter_exam' || this.$route.name === 'exam') {
            this.answer({
              random: signatureObj.random,
              signature: signatureObj.signature,
              answer: {no: this.exam.number, val: newValue},
              paper_id: this.$store.state.paperInfo.id,
              uid: this.$store.state.userInfo.id
            })
          }
        }
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .judge
    display flex
    flex-direction column
    font-size 14px
    .mint-radiolist-label
      display flex
      flex-direction row
      align-items center
      font-size 14px
      line-height 20px
      padding 10px 0
    .title, .choice
      font-size 14px
      padding 5px
</style>
