<template>
  <div class="tp-material">
    <div class="material">
      <mt-badge size="small">材料</mt-badge>
      <span v-html="exam.material"></span>
    </div>
    <div class="title">
      <mt-badge size="small">题目{{index+1}}</mt-badge>
      <span v-html="exam.title[index]"></span>
    </div>
    <div class="choice">
      <mt-radio title="请选择" v-model="value" :options='options'></mt-radio>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import {Badge, Toast} from 'mint-ui'
  import {mapGetters} from 'vuex'
  export default {
    name: 'material',
    components: {
      Badge,
      Toast
    },
    props: {
      exam: {
        type: Object,
        require: true
      },
      index: {
        type: Number,
        require: true
      },
      len: {
        type: Number,
        require: true
      },
      orientation: {
        type: Boolean,
        require: true
      },
      random: {
        type: Number,
        require: true
      }
    },
    computed: {
      ...mapGetters(['createSignature']),
      options () {
        let az = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
        let opt = []
        for (let i = 0; i < this.exam.option[this.index].length; i++) {
          opt.push({
            label: this.exam.option[this.index][i],
            value: az[i]
          })
        }
        return opt
      }
    },
    methods: {
      // 还原答案
      recoveryAnswer () {
        let data = this.$store.state.examState.userAnswer
        let bool = false
        for (let i = 0; i < data.length; i++) {
          if (data[i].no === this.exam.number) {
            for (let j = 0; j < data[i].val.length; j++) {
              if (+data[i].val[j]['key'] === this.index) {
                this.value = data[i].val[j].subval
                bool = true
              }
            }
          }
        }
        if (bool === false) {
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
    data () {
      return {
        value: ''
      }
    },
    created () {
      this.recoveryAnswer()
    },
    watch: {
      random () {
        let rightAnswer = (this.value.sort()).toString()
        if (this.exam.right_answer[this.index] === rightAnswer) {
          let instance = Toast({
            message: '恭喜，答对了',
            iconClass: 'mint-toast-icon mintui mintui-success'
          })
          setTimeout(function () {
            instance.close()
          }, 800)
        } else {
          this.$store.state.examState.examParse = true
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
      },
      exam () {
        if (this.orientation) {
          this.index = this.exam['title'].length - 1
        } else {
          this.index = 0
        }
        this.len = this.exam['title'].length
      },
      value (newValue) {
        if (newValue && newValue.length > 0) {
          let data = this.$store.state.examState.userAnswer
          let signatureObj = this.createSignature
          let change = false
          let hasExam = false
          let answerObj = ''
          for (let i = 0; i < data.length; i++) {
            if (data[i].no === this.exam.number) {
              hasExam = i
              for (let j = 0; j < data[i].val.length; j++) {
                if (+data[i].val[j]['key'] === this.index) {
                  data[i].val[this.index]['subval'] = newValue
                  change = true
                  break
                }
              }
              answerObj = data[hasExam]
            }
            break
          }
          if (change === false) {
            if (hasExam !== false) {
              data[hasExam].val.push({
                key: this.index,
                subval: newValue
              })
              answerObj = data[hasExam]
            } else {
              answerObj = {
                no: this.exam.number,
                val: [
                  {key: this.index, subval: newValue}
                ]
              }
              data.push({
                no: this.exam.number,
                val: [
                  {key: this.index, subval: newValue}
                ]
              })
            }
          }
          this.$store.commit('saveAnswer', data)
          if (this.$route.name === 'chapter_exam' || this.$route.name === 'exam') {
            this.answer({
              random: signatureObj.random,
              signature: signatureObj.signature,
              answer: answerObj,
              paper_id: this.$store.state.paperInfo.id,
              uid: this.$store.state.userInfo.id
            })
          }
        }
      },
      index () {
        this.recoveryAnswer()
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .tp-material
    flex: 1
    padding 5px
    font-size 14px
    .mint-checklist-label
      display flex
      flex-direction row
      align-items center
      line-height 20px
      padding 10px 0
      font-size 14px
    .title, .choice
      padding 5px
</style>
