<template>
  <div class="app-tab-wrapper">
    <div class="tab-scroll" ref="tabScroll" v-if="partName">
      <ul class="app-tab" :style="{width: 1.2 * headOptionsLen + 'rem'}">
        <li v-for="(val, key) in partName" @click.stop="selectTab(key, $event)" :key="key" :class="{active: actPart === key}">{{val}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import IScroll from 'iscroll'

export default {
  props: {
    actPart: {
      type: String,
      default () {
        return 'bg'
      }
    }
  },
  data () {
    return {
      partName: { // 部件中文名
        bg: '背景',
        bgColor: '背景颜色',
        body: '身体',
        cloth: '衣服',
        face: '脸型',
        faceColor: '脸形变色',
        eye: '眼睛',
        brow: '眉毛',
        nose: '鼻子',
        mouth: '嘴巴',
        glasses: '眼镜',
        hair: '头发',
        hairColor: '头发变色',
        hand: '动作',
        cap: '帽子',
        prop: '饰品',
        suit: '套装'
      }
    }
  },
  computed: {
    headOptionsLen () {
      var len = 0
      /* eslint-disable no-unused-vars */
      for (var item in this.partName) {
        len++
      }
      return len
    }
  },
  mounted () {
    setTimeout(() => {
      this._initScroll()
    }, 2000)
  },
  methods: {
    selectTab (key, event) {
      this.tabScroll.scrollToElement(event.target, 400, true)

      this.$emit('select-tab', key, event)
    },
    // 注册滚动
    _initScroll () {
      // 注册tab切滚动
      this.tabScroll = new IScroll(this.$refs.tabScroll, {
        click: true,
        scrollX: true,
        scrollY: false,
        disablePointer: true,
        disableTouch: false,
        disableMouse: false
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import "../../assets/stylus/variable"
  @import "../../assets/stylus/mixin"

  .app-tab-wrapper
    position absolute
    left 20px
    top 7.5rem
    z-index 2
    width -webkit-calc(100% - 40px)
    width calc(100% - 40px)
    height 1rem
    margin-top -0.5rem
    background #fff
    -webkit-box-shadow 0rem 0.1rem 0.2rem rgba(0,0,0,0.1)
    box-shadow 0rem 0.1rem 0.2rem rgba(0,0,0,0.1)
    .tab-scroll
      position absolute
      width 100%
      height 100%
      overflow hidden
      -webkit-mask -webkit-linear-gradient(left, rgba(255,255,255,0) 10px,  rgba(255,255,255,1) 30px,  rgba(255,255,255,1) -webkit-calc(100% - 30px), rgba(255,255,255,0) -webkit-calc(100% - 10px))
      touch-action none
      .app-tab
        display flex
        padding 0 10px
        li
          position relative
          flex 1
          height 1rem
          cursor pointer
          text-align center
          line-height 1rem
          font-size $font-size-medium-x
          color #999
          &.active
            color #78bdff
            &:after
              position absolute
              left 0.3rem
              bottom 0.2rem
              width 0.6rem
              border-bottom 2px #78bdff solid
              content ''
  #app.ios .app-tab-wrapper
    background rgba(255,255,255,0.8)
    -webkit-backdrop-filter blur(10px)
</style>
