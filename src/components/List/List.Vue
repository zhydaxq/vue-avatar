<template>
  <div class="app-list-wrapper" ref="appListWrapper">
    <div class="list-scroll" ref="listScroll">
      <!-- 部件 -->
      <ul v-if="actPart.indexOf('Color') === -1 && actPart !=='suit'" class="app-list" :class="actPart + '-list'" :style="{height:
        2.5 * listLen + 'rem'}">
        <li v-for="(item, index) in partList" class="app-item" :class="{active: index === actIndex}" :key="index" @click="selectItem(item, index)">
          <img v-if="index === 0" src="../../assets/images/restore.png" @click.stop="restoreItem" alt="" lazy="loaded">
          <img v-else v-lazy="item" :key="item" alt="">
        </li>
      </ul>
      <!-- 套装 -->
      <ul v-else-if="actPart === 'suit'" class="app-list" :class="actPart + '-list'" :style="{height: 2.5 * listLen + 'rem'}">
        <li @click="restoreSuit" class="app-item">
          <img src="../../assets/images/restore.png" alt="" lazy="loaded">
        </li>
        <li v-for="(val, key, index) in partList" :key="key" class="app-item" :class="{active: index === actIndex}" @click="selectSuit(val)">
          <img v-lazy="val.thumb" :key="val.thumb" alt="">
        </li>
      </ul>
      <!-- 颜色 -->
      <ul v-else class="app-list color-list">
        <ColorPicker @select-color="selectColor" :color-hsl="partList"></ColorPicker>
      </ul>
    </div>
  </div>
</template>

<script>
import IScroll from 'iscroll/build/iscroll-probe'
import ColorPicker from '@/base/ColorPicker/ColorPicker'

export default {
  props: {
    partList: {
      default () {
        return []
      }
    },
    actPart: {
      type: String,
      default () {
        return 'bg'
      }
    }
  },
  data () {
    return {
      actIndex: -1
    }
  },
  computed: {
    listLen () {
      var len = 0

      if (this.actPart.indexOf('Color') !== -1) {
        return len
      }
      if (this.actPart === 'suit') {
        /* eslint-disable no-unused-vars */
        for (var item in this.partList) {
          len++
        }
        len = Math.ceil((len + 1) / 3)
        return len
      }
      len = Math.ceil(this.partList.length / 3)
      return len
    }
  },
  watch: {
    partList () {
      setTimeout(() => {
        this.refresh()
        if (!document.querySelector('.app-item.active')) {
          this.listScroll && this.listScroll.scrollTo(0, 0)
        } else {
          this.listScroll && this.listScroll.scrollToElement(document.querySelector('.app-item.active'), 0, true, true)
        }
      }, 100)
    },
    actIndex (index) {
      this.actIndexes[this.actPart] = index
    },
    actPart (tabItem) {
      this.actIndex = this.actIndexes[tabItem]
    }
  },
  created () {
    // 初始化索引集合
    this.actIndexes = {}
  },
  mounted () {
    setTimeout(() => {
      this._initScroll()
    }, 2000)
  },
  methods: {
    selectItem (item, index) {
      this.actIndex = index
      this.$emit('select-item', this.actPart, item)
    },
    _initScroll () {
      // 注册列表滚动
      this.listScroll = new IScroll(this.$refs.listScroll, {
        click: true,
        scrollX: false,
        scrollY: true,
        disablePointer: true,
        disableTouch: false,
        disableMouse: false,
        mouseWheel: true,
        probeType: 3
      })
      // 监听列表滚动
      var that = this
      this.listScroll.on('scroll', function () {
        that.$emit('list-scroll', this)
      })
      this.listScroll.on('scrollEnd', function () {
        that.$emit('list-scroll-end', this)
      })
    },
    selectColor (options) {
      this.$emit('select-color', options.colorHsl)
    },
    // 选择套装
    selectSuit (options) {
      this.$emit('select-suit', options)
    },
    restoreItem () {
      this.$emit('restore-item')
    },
    restoreSuit () {
      this.$emit('restore-suit')
    },
    refresh () {
      this.listScroll && this.listScroll.refresh()
    }
  },
  components: {
    ColorPicker
  }
}
</script>

<style lang="stylus" scoped>
  @import "../../assets/stylus/variable"
  @import "../../assets/stylus/mixin"

  .app-list-wrapper
    position absolute
    bottom 0
    width 100%
    height -webkit-calc(100% - 8.5rem)
    height calc(100% - 8.5rem)
    background $color-background-l
    overflow hidden
    .list-scroll
      position absolute
      width 100%
      height 100%
      overflow hidden
      touch-action none
    .app-list
      width 100%
      .app-item
        position relative
        float left
        width 2.5rem
        height 2.5rem
        border 1px #f0f4f5 solid
        overflow hidden
        cursor pointer
        &:nth-child(-n+3)
          border-top none
        &:nth-child(3n+1)
          border-left:none
        &:nth-child(3n+2)
          border-left:none
          border-right:none
        &:nth-child(3n+3)
          border-right:none
        &:nth-child(6n+4), &:nth-child(6n+5),&:nth-child(6n+6)
          border-bottom:none
          border-top:none
        &:nth-child(3n+2):nth-last-child(1)
          border-right 1px #f0f4f5 solid
        &:nth-child(6n+6):nth-last-child(3)
          border-bottom 1px #f0f4f5 solid
        &:nth-child(6n+6):nth-last-child(2)
          border-bottom 1px #f0f4f5 solid
        &:nth-child(6n+5):nth-last-child(3)
          border-bottom 1px #f0f4f5 solid
        &.active:after
          position absolute
          left 0
          top 0
          right 0
          bottom 0
          border 2px #7cc2fd solid
          content ''
        img
          position absolute
          left 0
          top 0
          width auto
          height 100%
          transform-origin 50% 100%

    .body-list
      .app-item:nth-child(n+2) img
        transform scale(1.8,1.8)
        background-size 55% 55%
    .cloth-list
      .app-item:nth-child(n+2) img
        transform scale(1.8,1.8)
        background-size 55% 55%
    .nose-list
      .app-item:nth-child(n+2) img
        transform-origin 50% 70%
        transform scale(3,3)
        background-position center 68%
        background-size 30% 30%
    .mouth-list
      .app-item:nth-child(n+2) img
        transform-origin 50% 85%
        transform scale(3,3)
        background-position center 68%
        background-size 30% 30%
    .color-list
      width 100%
      height 100%
</style>
