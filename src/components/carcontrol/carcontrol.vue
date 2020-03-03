<template>
  <div class="carcontrol">
    <transition name="fade">
      <div class="car-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCar">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="car-count"  v-show="food.count>0">{{food.count}}</div>
    <div class="car-add icon-add_circle" @click.stop.prevent="addCar"></div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCar(event) {
      if(!this.food.count) {
        Vue.set(this.food, 'count', 1)
      }else {
        this.food.count++
      }
      // this.$root.eventHub.$emit('car.add', event.target)
      this.$emit('caradd', event.target)
    },
    decreaseCar() {
      if(this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus">
  .carcontrol
    font-size 0
    .car-decrease, .car-add
      display inline-block
      padding 6px
      &.fade-enter-active, &.fade-leave-active
          transition all 0.4s linear
      &.fade-enter, &.fade-leave-to
        opacity 0
        transform: translateX(24px) rotate(180deg)  // transform属性写在一起,否则下面的属性会把它覆盖掉
      .inner
        line-height 24px
        font-size 24px
        color rgb(0, 160, 220)
    .car-count
      display inline-block
      vertical-align top
      width 12px
      padding-top 6px 
      line-height 24px
      text-align center
      font-size 10px
      color: rgb(147, 153, 159)
    .car-add
      display inline-block
      padding 6px
      line-height 24px
      font-size 24px
      color rgb(0, 160, 220)
      
</style>
