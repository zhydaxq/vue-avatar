<template>
  <div class="app-head" ref="appHead" @click="headTap">
    <canvas id="canvas" class="canvas" width="568" height="568">
      <span>不支持canvas浏览器</span>
    </canvas>
    <img class="canvas-cover" ref="canvasCover" src="" alt="">
    <span class="down-tips">长按保存图片</span>
    <span class="rondom" @click.stop="rondomSelect"></span>
  </div>
</template>

<script>
import $ from 'jquery'
import '@/assets/js/plugins/jcanvas'
import {hslToRgb} from '@/assets/js/plugins/hsl2rgb'

export default {
  props: {
    headOptions: {
      type: Object,
      default () {
        return {}
      }
    }
  },
  computed: {
    // 是否带背景头发
    hasBgHair () {
      var url = this.headOptions.hair
      if (url.indexOf('data:image/') === -1) {
        var reg = new RegExp(/^.*\?.*hasBgHair=([^&].+)/g)
        /* eslint-disable vue/no-side-effects-in-computed-properties */
        /* eslint-disable no-eval */
        this.lastHasBgHair = reg.test(url) ? eval(url.replace(reg, '$1').toLowerCase()) : false
      }
      return this.lastHasBgHair
    },
    bgColor () {
      return this.headOptions.bgColor
    },
    faceColor () {
      return this.headOptions.faceColor
    },
    hairColor () {
      return this.headOptions.hairColor
    }
  },
  watch: {
    headOptions: {
      handler () {
        this.removeCanvas()
        this.drawCanvas()
      },
      deep: true
    },
    bgColor (colorHsl) {
      this._selectColor(colorHsl, this.headOptions.bg)
    },
    faceColor (colorHsl) {
      this._selectColor(colorHsl, this.headOptions.face)
    },
    hairColor (colorHsl) {
      this._selectColor(colorHsl, this.headOptions.hair)
    }
  },
  created () {
    Object.assign($.jCanvas.defaults, {
      type: 'image',
      crossOrigin: 'Anonymous',
      fromCenter: false,
      inDegrees: false,
      x: 0,
      y: 0,
      height: 568
    })
  },
  mounted () {
    this.drawCanvas()
  },
  methods: {
    rondomSelect () {
      this.$emit('rondom-select')
    },
    headTap () {
      this.drawCanvas()
      this.$emit('head-tap')
    },
    drawCanvas () {
      var that = this
      var options = this.headOptions
      $('.canvas').addLayer({ // 绘制背景
        name: 'bgLayer',
        source: options.bg,
        width: 568,
        height: 568
      }).addLayer({ // 绘制后背头发
        name: 'hairBgLayer',
        source: options.hair,
        x: -568,
        width: 1136,
        visible: this.hasBgHair
      }).addLayer({ // 绘制身体
        name: 'bodyLayer',
        source: options.body
      }).addLayer({ // 绘制衣服
        name: 'clothLayer',
        source: options.cloth
      }).addLayer({ // 绘制脸型
        name: 'faceLayer',
        source: options.face
      }).addLayer({ // 绘制眼睛
        name: 'eyeLayer',
        source: options.eye
      }).addLayer({ // 绘制眉毛
        name: 'browLayer',
        source: options.brow
      }).addLayer({ // 绘制鼻子
        name: 'noseLayer',
        source: options.nose
      }).addLayer({ // 绘制嘴巴
        name: 'mouthLayer',
        source: options.mouth
      }).addLayer({ // 绘制眼镜
        name: 'glassesLayer',
        source: options.glasses
      }).addLayer({ // 绘制头发
        name: 'hairLayer',
        source: options.hair,
        x: 0,
        width: this.hasBgHair ? 1136 : 568
      }).addLayer({ // 绘制帽子
        name: 'capLayer',
        source: options.cap
      }).addLayer({ // 绘制动作
        name: 'handLayer',
        source: options.hand
      }).addLayer({ // 绘制饰品
        name: 'propLayer',
        source: options.prop
      }).addLayer({ // 绘制水印
        name: 'markLayer',
        source: 'https://avatarData.zhydaxq.com/images/other/mark.png'
      }).drawLayers({
        complete () {
          that.canvasLoaded()
        }
      })
    },
    // 清空Canvas
    removeCanvas () {
      $('.canvas').removeLayers()
    },
    canvasLoaded () {
      var imgData = $('.canvas').getCanvasImage('png')
      $('.canvas-cover').attr('src', imgData)
    },
    // 获取Canvas数据
    _getImgData (imgUrl) {
      var canvas = $('.canvas')[0].getContext('2d')
      var img = new Image()
      img.src = imgUrl
      return new Promise((resolve, reject) => {
        img.onload = () => {
          canvas.clearRect(0, 0, 568, 568)
          canvas.drawImage(img, 0, 0, 568, 568)
          var imageData = canvas.getImageData(0, 0, 568, 568)
          resolve(imageData)
        }
      })
    },
    // color-picker组件选色事件
    _selectColor (colorHsl, targetNode) {
      var rgbArr = hslToRgb.apply(this, colorHsl)

      this._getImgData(targetNode).then(imgData => {
        var oldU8a = imgData.data
        var newImgdata = $('.canvas')[0].getContext('2d').createImageData(568, 568)
        var newU8a = newImgdata.data
        for (var i = 0; i < oldU8a.length; i += 4) {
          if ((oldU8a[i] === 6 && oldU8a[i + 1] === 0 && oldU8a[i + 2] === 1) || oldU8a[i + 3] < 255) {
            newU8a[i] = oldU8a[i]
            newU8a[i + 1] = oldU8a[i + 1]
            newU8a[i + 2] = oldU8a[i + 2]
            newU8a[i + 3] = oldU8a[i + 3]
          } else {
            newU8a[i] = rgbArr[0]
            newU8a[i + 1] = rgbArr[1]
            newU8a[i + 2] = rgbArr[2]
            newU8a[i + 3] = 255
          }
        }

        $('.canvas')[0].getContext('2d').putImageData(newImgdata, 0, 0)
        var colorImg = $('.canvas')[0].toDataURL()
        this.$emit('select-color-img', colorImg)
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import "../../assets/stylus/variable"
  @import "../../assets/stylus/mixin"

  .app-head
    position absolute
    width 7.5rem
    height 7.5rem
    overflow hidden
    .canvas, .canvas-cover
      position absolute
      left 50%
      top 50%
      width 7.5rem
      height 7.5rem
      transform translate(-50%, -50%)
    .canvas
      pointer-events none
      visibility hidden
    .canvas-cover
      -webkit-user-select:auto
    .down-tips
      position absolute
      top 20px
      left 20px
      z-index 3
      width 1.8rem
      height 0.4rem
      font-size $font-size-medium
      text-align center
      line-height 0.4rem
      background rgba(0,0,0,0.2)
      color #fff
      pointer-events none
    .rondom
      position absolute
      right 20px
      top 20px
      width 0.8rem
      height 0.8rem
      background url('../../assets/images/random.png')
      background-size 100% 100%
</style>
