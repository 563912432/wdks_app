<template>
  <div class="member">
    <memberHeader :userInfo="userInfo"></memberHeader>
    <memberMenu></memberMenu>
  </div>
</template>
<script>
  import {MessageBox} from 'mint-ui' // MessageBox
  import memberHeader from '@/components/memberHeader'
  import memberMenu from '@/components/memberMenu'
  export default {
    name: 'member',
    components: {
      memberHeader,
      memberMenu
    },
    data () {
      return {
        userInfo: {headimg: ''},
        showMsgAlert: false
      }
    },
    created () {
      this.$store.commit('setSelected', '4')
      this.userInfo = this.$store.state.userInfo
      this.showAd()
    },
    methods: {
      showAd () {
        // 判断是否有未读消息，弹窗提醒
        if (this.$store.state.adShowSwitch) {
          if (this.userInfo.unread_msg_num > 0) {
            // 弹窗提示未读信息
            let that = this
            MessageBox({
              title: '',
              message: '您有' + this.userInfo.unread_msg_num + '条未读信息',
              showCancelButton: true,
              cancelButtonText: '以后再看',
              showConfirmButton: true,
              confirmButtonText: '前往查看'
            }).then(action => {
              if (action === 'confirm') {
                that.$router.push({path: '/message'})
              }
            })
          }
          this.$store.commit('setAdShowSwitch', false)
        }
      }
    }
  }
</script>

<style lang="stylus" rel='stylesheet/stylus'>
  .member
    flex 1
    display flex
    flex-direction column
</style>
