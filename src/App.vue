<template>
  <div id="app" :class="device">
    <Loading :preLoadArr="preLoadArr"></Loading>
    <StartPage :isShow="startPageShow" @start-go="StartGO"></StartPage>
    <Avatar
      :headOptions="headOptions"
      @select-color-img="selectColorImg"
      @head-tap="headTap"
      @rondom-select="randomSelect"
      ref="appHead"
    ></Avatar>
    <Tab
      :act-part="actPart"
      @select-tab="selectTab"
      ref="appTabWrapper"
    ></Tab>
    <List
      :partList="partList"
      :act-part="actPart"
      @select-item="selectItem"
      @select-suit="selectSuit"
      @select-color="selectColor"
      @restore-item="restoreItem"
      @restore-suit="restoreSuit"
      @list-scroll="listScroll"
      @list-scroll-end="listScrollEnd"
      ref="appListWrapper"
    ></List>
  </div>
</template>

<script>
import $ from 'jquery'
import Avatar from '@/components/Avatar/Avatar'
import Tab from '@/components/Tab/Tab'
import List from '@/components/List/List'
import Loading from '@/base/Loading/Loading'
import StartPage from '@/base/StartPage/StartPage'

const ERR_OK = 0
const UA = window.navigator.userAgent

export default {
  name: 'App',
  data () {
    return {
      // headOrigin: {
      //   bg: 'https://avatarData.zhydaxq.com/images/bg/bg_000.jpg',
      //   bgColor: [180, '100%', '50%'],
      //   body: 'https://avatarData.zhydaxq.com/images/body/body_000.png',
      //   cloth: 'https://avatarData.zhydaxq.com/images/cloth/cloth_002.png',
      //   face: 'https://avatarData.zhydaxq.com/images/face/face_000.png',
      //   faceColor: [180, '100%', '50%'],
      //   eye: 'https://avatarData.zhydaxq.com/images/eye/eye_000.png',
      //   brow: 'https://avatarData.zhydaxq.com/images/brow/brow_000.png',
      //   nose: 'https://avatarData.zhydaxq.com/images/nose/nose_000.png',
      //   mouth: 'https://avatarData.zhydaxq.com/images/mouth/mouth_000.png',
      //   glasses: 'https://avatarData.zhydaxq.com/images/glasses/glasses_000.png',
      //   hair: 'https://avatarData.zhydaxq.com/images/hair/hair_008.png',
      //   hairColor: [180, '100%', '50%'],
      //   cap: 'https://avatarData.zhydaxq.com/images/cap/cap_000.png',
      //   hand: 'https://avatarData.zhydaxq.com/images/hand/hand_000.png',
      //   prop: 'https://avatarData.zhydaxq.com/images/prop/prop_000.png',
      //   suit: {}
      // },
      headOrigin: {
        bg: 'https://avatarData.zhydaxq.com/images/bg/bg_019.jpg',
        bgColor: [180, '100%', '50%'],
        body: 'https://avatarData.zhydaxq.com/images/body/body_000.png',
        cloth: 'https://avatarData.zhydaxq.com/images/cloth/cloth_022.png',
        face: 'https://avatarData.zhydaxq.com/images/face/face_005.png',
        faceColor: [180, '100%', '50%'],
        eye: 'https://avatarData.zhydaxq.com/images/eye/eye_016.png',
        brow: 'https://avatarData.zhydaxq.com/images/brow/brow_002.png',
        nose: 'https://avatarData.zhydaxq.com/images/nose/nose_000.png',
        mouth: 'https://avatarData.zhydaxq.com/images/mouth/mouth_013.png',
        glasses: 'https://avatarData.zhydaxq.com/images/glasses/glasses_000.png',
        hair: 'https://avatarData.zhydaxq.com/images/hair/hair_023.png',
        hairColor: [180, '100%', '50%'],
        cap: 'https://avatarData.zhydaxq.com/images/cap/cap_000.png',
        hand: 'https://avatarData.zhydaxq.com/images/hand/hand_006.png',
        prop: 'https://avatarData.zhydaxq.com/images/prop/prop_022.png',
        suit: {}
      },
      headOptions: {},
      partList: [],
      actPart: 'bg',
      actIndex: -1,
      preLoadArr: [
        'https://avatarData.zhydaxq.com/images/other/loading.jpg',
        'https://avatarData.zhydaxq.com/images/other/start.png',

        // 默认展示
        'https://avatarData.zhydaxq.com/images/bg/bg_000.jpg',
        'https://avatarData.zhydaxq.com/images/body/body_000.png',
        'https://avatarData.zhydaxq.com/images/cloth/cloth_002.png',
        'https://avatarData.zhydaxq.com/images/face/face_000.png',
        'https://avatarData.zhydaxq.com/images/eye/eye_000.png',
        'https://avatarData.zhydaxq.com/images/brow/brow_000.png',
        'https://avatarData.zhydaxq.com/images/nose/nose_000.png',
        'https://avatarData.zhydaxq.com/images/mouth/mouth_000.png',
        'https://avatarData.zhydaxq.com/images/glasses/glasses_000.png',
        'https://avatarData.zhydaxq.com/images/hair/hair_008.png',
        'https://avatarData.zhydaxq.com/images/cap/cap_000.png',
        'https://avatarData.zhydaxq.com/images/hand/hand_000.png',
        'https://avatarData.zhydaxq.com/images/prop/prop_000.png',

        // 活动默认展示
        'https://avatarData.zhydaxq.com/images/suit/qixiBoy.jpg',
        'https://avatarData.zhydaxq.com/images/bg/bg_019.jpg',
        'https://avatarData.zhydaxq.com/images/cloth/cloth_022.png',
        'https://avatarData.zhydaxq.com/images/face/face_005.png',
        'https://avatarData.zhydaxq.com/images/eye/eye_016.png',
        'https://avatarData.zhydaxq.com/images/brow/brow_002.png',
        'https://avatarData.zhydaxq.com/images/mouth/mouth_013.png',
        'https://avatarData.zhydaxq.com/images/hair/hair_023.png',
        'https://avatarData.zhydaxq.com/images/hand/hand_006.png',
        'https://avatarData.zhydaxq.com/images/prop/prop_022.png'
      ],
      startPageShow: true,
      device: {}
    }
  },
  created () {
    // 初始化默认数据
    this.headOptions = Object.assign({}, this.headOrigin)

    // 获取query
    this.searches = {}
    window.location.search.substring(1).split('&').forEach(item => {
      let ret = item.split('=')
      this.searches[ret[0]] = ret[1]
    })

    // 获取设备型号
    this.device = {
      app: !!UA.match(/SNEBUY-APP;?/i),
      lite: !!UA.match(/SNLITE-WAP/i),
      android: !!UA.match(/android/i),
      ios: !!UA.match(/(iPhone|iPod|iPad);?/i),
      iphonex: !!UA.match(/(iPhone|iPod|iPad);?/i) && (screen.height === 812 && screen.width === 375),
      wx: !!UA.match(/MicroMessenger/i)
    }

    // ajax获取接口数据
    $.ajax('/api/data').then((response) => {
      if (response.errno === ERR_OK) {
        this.avatarData = response.data
        this.partList = this.avatarData[this.actPart]
        // 设置节日主题
        if (this.searches && this.searches.custom === 'qixijie') {
          this.headOptions = Object.assign(this.headOptions, this.headOrigin, this.avatarData.suit.qixiBoy)
        } else {
          this.headOptions = Object.assign(this.headOptions, this.headOrigin)
        }
      }
    }, (response) => {
      console.log('error')
    })

    this.touch = {
      moveY: 0,
      endY: 0,
      // 列表滚动是否可以重置
      isListRefresh: false,
      isStop: false
    }
  },
  mounted () {
    // 获取DOM初始值
    this.appHeadHeightOrigin = parseInt(window.getComputedStyle(this.$refs.appHead.$el).height)
    this.appTabWrapperHeightOrigin = parseInt(window.getComputedStyle(this.$refs.appTabWrapper.$el).height)
    this.appListWrapperHeightOrigin = parseInt(window.getComputedStyle(this.$refs.appListWrapper.$el).height)
  },
  methods: {
    selectTab (key) {
      this.partList = key.match(/Color$/) ? this.headOptions[key] : this.avatarData[key]
      this.actPart = key
    },
    selectItem (actPart, item) {
      this.headOptions[actPart] = item
    },
    selectSuit (options) {
      this.headOptions = Object.assign(this.headOptions, this.headOrigin, options)
    },
    selectColor (hslArr) {
      this.headOptions[this.actPart] = hslArr
    },
    selectColorImg (img) {
      this.headOptions[this.actPart.replace(/Color$/g, '')] = img
    },
    // 恢复初始部件
    restoreItem () {
      this.headOptions[this.actPart] = this.avatarData[this.actPart][0]
    },
    // 恢复套装
    restoreSuit () {
      this.headOptions = Object.assign(this.headOptions, this.headOrigin)
    },
    listScroll (e) {
      if (this.touch.isStop) { // 是否停止滚动
        return
      }

      // console.log(`e.maxScrollY: ${e.maxScrollY}, e.distY: ${e.distY}, e.y: ${e.y}, ${e.maxScrollY/2 - e.y}`)
      // console.log((e.maxScrollY - e.y) * 0.5))
      // 元素上下位移值，最大值为tab切高度，最小值为0
      // this.touch.moveY = Math.min(Math.max(0 - this.appTabWrapperHeightOrigin, (this.touch.endY - (e.maxScrollY / 2 - e.y) * 0.5)), 0)
      this.touch.moveY = Math.min(Math.max(0 - this.appTabWrapperHeightOrigin, (this.touch.endY + e.distY * 0.5)), 0)

      // 头部高度调整
      this.$refs.appHead.$el.style.height = this.appHeadHeightOrigin + this.touch.moveY + 'px'
      this.$refs.appHead.$el.style.transition = null
      // tab切高度调整
      this.$refs.appTabWrapper.$el.style.transform = 'translate(0, ' + this.touch.moveY + 'px)'
      this.$refs.appTabWrapper.$el.style.transition = null
      // 部件高度调整
      this.$refs.appListWrapper.$el.style.height = this.appListWrapperHeightOrigin - this.touch.moveY * 2 + 'px'
      this.$refs.appListWrapper.$el.style.transition = null

      // 重置滚动
      var appListWrapperOuterHeight = this.appListWrapperHeightOrigin + this.appTabWrapperHeightOrigin * 2
      var appListWrapperInnerHeight = this.$refs.appListWrapper.$el.childNodes[0].offsetHeight
      if (this.touch.moveY === 0 - this.appTabWrapperHeightOrigin && appListWrapperOuterHeight <= appListWrapperInnerHeight && !this.touch.isListRefresh) { // 向上滚动到高度最大时，同时滚动元素大于外包元素时
        this.touch.isListRefresh = true
        this.$refs.appListWrapper.refresh()
      }
    },
    listScrollEnd (e) {
      // 开启滚动
      this.touch.isStop = false
      // //
      this.touch.isListRefresh = false
      // //
      this.touch.endY = this.touch.moveY
    },
    StartGO () {
      this.startPageShow = false
    },
    headTap () {
      // 关闭滚动
      this.touch.isStop = true

      // 重置各元素初始样式
      this.touch.moveY = this.touch.endY = 0
      this.$refs.appHead.$el.style.height = this.appHeadHeightOrigin + 'px'
      this.$refs.appHead.$el.style.transition = 'height 0.3s'
      this.$refs.appTabWrapper.$el.style.transform = 'translate(0,0)'
      this.$refs.appTabWrapper.$el.style.transition = 'transform 0.3s'
      this.$refs.appListWrapper.$el.style.height = this.appListWrapperHeightOrigin + 'px'
      this.$refs.appListWrapper.$el.style.transition = 'height 0.3s'
    },
    // 随机选择部件
    randomSelect () {
      var result = {}
      Object.entries(this.avatarData).forEach((arr) => {
        let key = arr[0]
        let val = arr[1]
        if (!key.match(/(suit|Color)\b$/)) {
          // 取随机数，调高0的概率
          let random = Math.ceil(Math.random() * (val.length - 1 + 10))
          random = Math.max(0, random - 10)

          if (key === 'prop') {
            random = 0
          }

          result[key] = val[random]
          // console.log(`ket: ${key}, random: ${random}, val: ${val[random]}, len: ${val.length}`)
        }
      })
      this.headOptions = Object.assign(this.headOptions, result)
    }
  },
  components: {
    Avatar,
    Tab,
    List,
    Loading,
    StartPage
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  position: absolute;
  left: 50%;
  width: 100%;
  height: 100%;
  max-width: 7.5rem;
  transform: translate(-50%,0);
  background: #f0f4f5;
}
</style>
