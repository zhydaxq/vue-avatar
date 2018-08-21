<template>
  <transition name="fade" :duration="500">
    <div class="loading" v-if="isShow">
      <div class="loading-icon">
        <i class="dot"></i>
        <i class="dot"></i>
        <i class="dot"></i>
      </div>
      <span class="loading-text">加载中... {{precent}}%</span>
    </div>
  </transition>
</template>

<script>
import {imgPreLoad} from '@/assets/js/plugins/imgPreLoad'

export default {
  props: {
    preLoadArr: {
      type: Array,
      default () {
        return []
      }
    }
  },
  data () {
    return {
      isShow: true,
      precent: 0
    }
  },
  created () {
    imgPreLoad(this.preLoadArr, (percentage) => {
      var percentT = percentage * 100
      this.precent = parseInt(percentT)
      if (percentage === 1) {
        // 移除加载动画
        setTimeout(() => {
          this.isShow = false
        }, 2000)
      }
    }, 3000)
  }
}
</script>

<style lang="stylus" scoped>
.loading {
  position: fixed;
  z-index: 9999;
  width: 100%;
  height: 100%;
  text-align: center;
  background: url(../../assets/images/loading.jpg) no-repeat #77bdff;
  background-size: auto 100%;
  background-position: center;
  font-size: 0;
  opacity: 1;
}
.loading.removing {
  opacity: 0;
  -o-transition: opacity .5s ease-in;
  transition: opacity .5s ease-in;
  -webkit-transition: opacity .5s ease-in;
}
.loading-icon {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 40px;
  height: 40px;
  margin: 1.5rem 0 0 -20px;
  text-align: center;
  background-size: 100% 100%;
  overflow: hidden;
}
.loading-icon:after {
  display: inline-block;
  width: 20px;
  height: 20px;
  margin-top: 8px;
  background: #fff;
  border-radius: 2px;
  -webkit-animation: loading-icon-after 0.5s ease 0s infinite;
          animation: loading-icon-after 0.5s ease 0s infinite;
  content: "";
}
@-webkit-keyframes loading-icon-after {
  0% {
    -webkit-transform: rotate(0deg);
            transform: rotate(0deg);
    -webkit-transform-origin: 100% 100%;
            transform-origin: 100% 100%;
  }
  100% {
    -webkit-transform: translate3d(-20px,0,0) rotate(90deg);
            transform: translate3d(-20px,0,0) rotate(90deg);
    -webkit-transform-origin: 100% 100%;
            transform-origin: 100% 100%;
  }
}
@keyframes loading-icon-after {
  0% {
    -webkit-transform: rotate(0deg);
            transform: rotate(0deg);
    -webkit-transform-origin: 100% 100%;
            transform-origin: 100% 100%;
  }
  100% {
    -webkit-transform: translate3d(-20px,0,0) rotate(90deg);
            transform: translate3d(-20px,0,0) rotate(90deg);
    -webkit-transform-origin: 100% 100%;
            transform-origin: 100% 100%;
  }
}
.loading-icon .dot {
  position: absolute;
  left: 8px;
  bottom: 5px;
  width: 4px;
  height: 4px;
  background: #fff;
  border-radius: 1px;
  content: "";
  -webkit-animation: loading-icon-dot 0.5s ease 0s infinite;
          animation: loading-icon-dot 0.5s ease 0s infinite;
}
.loading-icon .dot:nth-child(2) {
  left: 28px;
}
.loading-icon .dot:nth-child(3) {
  left: 48px;
}
@-webkit-keyframes loading-icon-dot {
  0% {
    -webkit-transform: translate3d(0,0,0);
            transform: translate3d(0,0,0);
  }
  100% {
    -webkit-transform: translate3d(-20px,0,0);
            transform: translate3d(-20px,0,0);
  }
}
@keyframes loading-icon-dot {
  0% {
    -webkit-transform: translate3d(0,0,0);
            transform: translate3d(0,0,0);
  }
  100% {
    -webkit-transform: translate3d(-20px,0,0);
            transform: translate3d(-20px,0,0);
  }
}
.loading-text {
  position: absolute;
  left: 0;
  top: 50%;
  width: 100%;
  margin-top: -webkit-calc(1.5rem + 40px);
  margin-top: calc(1.5rem + 40px);
  font-size: 12px;
  line-height: 3em;
  color: #fff;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0
}
</style>
