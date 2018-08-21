<template>
  <ul class="color-picker" ref="colorPicker">
    <li class="color-bar color-hue" style="background: -webkit-linear-gradient(left, hsl(0,100%,50%), hsl(60,100%,50%), hsl(120,100%,50%), hsl(180,100%,50%), hsl(240,100%,50%), hsl(300,100%,50%), hsl(360,100%,50%));"><span>色相</span><i @touchstart.prevent="pickerTouchStart" @touchmove.prevent="pickerTouchMove" @touchend="pickerTouchEnd" :style="{transform: 'translate('+ touch.wrapWidth * colorH/360 +'px,0)'}"></i></li>
    <li class="color-bar color-saturation" style="background: -webkit-linear-gradient(left, hsl(0,0%,50%), hsl(0,100%,50%));" :style="{background: '-webkit-linear-gradient(left, hsl(' + colorH + ',0%,50%), hsl(' + colorH + ',100%,50%))'}"><span>饱和度</span><i @touchstart.prevent="pickerTouchStart" @touchmove.prevent="pickerTouchMove" @touchend="pickerTouchEnd" :style="{transform: 'translate('+ touch.wrapWidth * colorS.replace('%', '')/100 +'px,0)'}"></i></li>
    <li class="color-bar color-lightness" style="background: -webkit-linear-gradient(left, hsl(0,100%,0%), hsl(0,100%,100%))"><span>明度</span><i @touchstart.prevent="pickerTouchStart" @touchmove.prevent="pickerTouchMove" @touchend="pickerTouchEnd" :style="{transform: 'translate('+ touch.wrapWidth * colorL.replace('%', '')/100 +'px,0)'}"></i></li>
  </ul>
</template>

<script>
import {hslToRgb} from '@/assets/js/plugins/hsl2rgb'

export default {
  data () {
    return {
      touch: {
        startX: 0,
        deltaX: 0,
        offsetWidth: 0,
        wrapWidth: 0
      },
      colorH: 180,
      colorS: '100%',
      colorL: '50%'
    }
  },
  props: {
    colorHsl: {
      type: Array,
      default () {
        return [180, '100%', '50%']
      }
    }
  },
  watch: {
    colorHsl (newColorHsl) {
      this.colorH = newColorHsl[0]
      this.colorS = newColorHsl[1]
      this.colorL = newColorHsl[2]
    }
  },
  created () {
    this.colorH = this.colorHsl[0]
    this.colorS = this.colorHsl[1]
    this.colorL = this.colorHsl[2]
  },
  mounted () {
    setTimeout(() => {
      this.touch.wrapWidth = this.$refs.colorPicker.clientWidth
    }, 50)
  },
  methods: {
    pickerTouchStart (e) {
      var el = e.target
      this.touch.offsetWidth = el.style.transform !== 'none' ? el.style.transform.replace(/^translate\((\d*.?\d*)px,\s*\d*.?\w*\)$/g, '$1') : 0
      this.touch.startX = e.touches[0].clientX
    },
    pickerTouchMove (e) {
      var el = e.target
      this.touch.deltaX = e.touches[0].clientX - this.touch.startX

      if (el.offsetParent.className.indexOf('color-hue') !== -1) {
        this.colorH = (Math.min(this.touch.wrapWidth, Math.max(0, (parseInt(this.touch.offsetWidth) + this.touch.deltaX))) / this.touch.wrapWidth) * 360
      }

      if (el.offsetParent.className.indexOf('color-saturation') !== -1) {
        this.colorS = (Math.min(this.touch.wrapWidth, Math.max(0, (parseInt(this.touch.offsetWidth) + this.touch.deltaX))) / this.touch.wrapWidth) * 100 + '%'
      }

      if (el.offsetParent.className.indexOf('color-lightness') !== -1) {
        this.colorL = (Math.min(this.touch.wrapWidth, Math.max(0, (parseInt(this.touch.offsetWidth) + this.touch.deltaX))) / this.touch.wrapWidth) * 100 + '%'
      }
    },
    pickerTouchEnd (e) {
      var rgb = hslToRgb(this.colorH, this.colorS, this.colorL)
      this.$emit('select-color', {
        colorRgb: rgb,
        colorHsl: [this.colorH, this.colorS, this.colorL]
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
.color-picker {
  position: relative;
  width: -webkit-calc(100% - 40px);
  width: calc(100% - 40px);
  height: 100%;
  margin-left: 20px;
}
.color-picker .color-bar {
  position: absolute;
  top: -webkit-calc(25% - 0.06rem);
  top: calc(25% - 0.06rem);
  width: 100%;
  height: 0.12rem;
  border: none;
  border-radius: 0.06rem;
  overflow: visible;
}
.color-picker .color-bar:nth-child(2) {
  top: -webkit-calc(50% - 0.06rem);
  top: calc(50% - 0.06rem);
}
.color-picker .color-bar:nth-child(3) {
  top: -webkit-calc(75% - 0.06rem);
  top: calc(75% - 0.06rem);
}
.color-bar span {
  display: inline-block;
  height: 0.22rem;
  line-height: 0.22rem;
  color: #aaa;
  font-size: 0.22rem;
  -webkit-transform: translate(0, -0.4rem);
      -ms-transform: translate(0, -0.4rem);
          transform: translate(0, -0.4rem);
}
.color-bar i {
  position: absolute;
  left: -0.45rem;
  top: -0.39rem;
  width: 0.3rem;
  height: 0.3rem;
  padding: 0.3rem;
  -webkit-box-sizing: content-box;
          box-sizing: content-box;
  font-size: 0;
}
.color-bar i:after {
  display: inline-block;
  width: 100%;
  height: 100%;
  background: #50b3e7;
  border-radius: 0.15rem;
  border: 0.02rem #fff solid;
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
  font-size: 0;
  content: '';
}
</style>
