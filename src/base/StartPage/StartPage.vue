<template>
  <transition name="fadedown" :duration="500">
    <div class="start-page" v-if="isShow" @click="startGO">
      <span class="start"></span>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    isShow: {
      type: Boolean,
      default () {
        return true
      }
    }
  },
  methods: {
    startGO () {
      this.$emit('start-go')
    }
  }
}
</script>

<style lang="stylus" scoped>
.start-page
  position fixed
  z-index 8888
  width 100%
  height 100%
  text-align center
  background url(../../assets/images/start.png) no-repeat #fff
  background-size contain
  background-position center center
  font-size 0
  .start
    position absolute
    left 0
    top 0
    right 0
    bottom 0
  &.fadedown-enter-active, &.fadedown-leave-active
    transition all .5s
  &.fadedown-enter, &.fadedown-leave-to
    opacity 0
    transform translate3d(0, 101%, 0)
</style>
