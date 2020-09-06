<template>
  <!-- <el-card class="admin-header"> -->
    <!-- <a href="/index">
      <img src="../../assets/img/icon/icon2.png" alt="" width="55px" style="float: left;margin-top: -5px;">
    </a>  animation-play-state: paused; :style="{'animation-play-state':animationShow}"-->
    <el-card>
      <div @click="playPause()" @mouseenter="enter" @mouseleave="leave" class="touxiang">
            <el-avatar :size="100" style="    z-index: 1;position: absolute;">
              <img :class="{pic:true,isPause:isPaused}"  :src="circleUrl" />
            </el-avatar>
            <i class="el-icon-video-pause myCamera" v-show="pauseShow"></i>
            <i class="el-icon-video-play myCamera" v-show="playShow"></i>
      </div>
      <span style="font-size: 32px;font-weight: bold;position:absolute;left: 150px;margin-top:20px;">白  卷</span>
      <i class="el-icon-switch-button" v-on:click="logout" style="font-size: 40px;float: right"></i>
      <audio :src="musicUrl" muted autoplay="autoplay" loop ref="music" hidden></audio>
    </el-card>
  <!-- </el-card> -->
</template>

<script>
  import {createRouter} from '../../router'
  export default {
    name: 'Header',
    data () {
      return {
        circleUrl: require('@/assets/img/icon/icon8.png'),
        musicUrl: require('@/assets/music/test.mp3'),
        playFlag: true,
        isPaused: false,
        pauseShow: false,
        playShow: false
      }
    },
    mounted () {
      var audio = this.$refs.music
      audio.play()
    },
    methods: {
      logout () {
        var _this = this
        this.$axios.get('/logout').then(resp => {
          if (resp && resp.data.code === 200) {
            _this.$store.commit('logout')
            _this.$router.replace('/index')
            // 清空路由，防止路由重复加载
            const newRouter = createRouter()
            _this.$router.matcher = newRouter.matcher
          }
        }).catch(failResponse => {})
      },
      playPause () {
        var audio = this.$refs.music
        if (audio !== null) {
          if (this.playFlag) {
            audio.pause()
            this.playFlag = false
            this.isPaused = true
            this.pauseShow = true
            this.playShow = false
            return
          }
          if (!this.playFlag) {
            audio.play()
            this.playFlag = true
            this.isPaused = false
            this.pauseShow = false
            this.playShow = true
          }
        }
      },
      enter: function () {
        if (this.playFlag) {
          this.playShow = true
          return
        }
        if (!this.playFlag) {
          this.pauseShow = true
        }
      },
      leave: function () {
        this.pauseShow = false
        this.playShow = false
      }
    }
  }
</script>

<style scoped>
  /* .admin-header {
    height: 120px;
    opacity: 0.85;
    line-height: 40px;
    min-width: 900px;
  } */
  .el-icon-switch-button {
    cursor: pointer;
    outline:0;
  }
  .pic {
    animation: rot 5s infinite linear;
    width: 100px;
    height: 100px
  }
  .isPause {
     animation-play-state: paused;
  }
  @keyframes rot {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
  .myCamera {
        font-size: 60px;
        vertical-align: top;
        line-height: 100px;
        height: 100px;
        width: 100px;
        background: rgba(0,0,0,.2);
        border-radius: 50px;
        color: rgba(255,255,255,.5);
        font-size: 50px;
        margin-top: -106px;
        z-index: 2;
        position: absolute;
        top: 147px;
    }
    .touxiang {
        width: 100px;
        height: 100px;
        margin: 0 auto;
        border-radius: 50px;
        cursor: pointer;
        margin-top: 20px;
    }
</style>
