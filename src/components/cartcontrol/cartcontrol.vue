<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count" @click="decreaseCart">
        <i class="inner icon-remove_circle_outline" ></i>
      </div>
    </transition>

    <div class="text">
      <span v-show="food.count">{{food.count}}</span>
    </div>
    <div class="cart-decrease" @click="addCount">
      <i class="inner icon-add_circle"></i>
    </div>
  </div>
</template>
<script type='text/ecmascript-6'>
import Vue from "vue";
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCount(event) {
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
      this.$emit('add', event.target)
    },
    decreaseCart() {
      if (this.food.count) {
        this.food.count--;
      }
    }
  }
};
</script>
<style scoped lang='stylus' rel='stylesheet/stylus'>
.cartcontrol 
  width: 72px
  height: 24px
  display: flex
  justify-content: space-between
  align-content: center
  .cart-decrease 
    width: 24px;
    height: 24px;
    font-size: 24px;
    color: rgb(0, 160, 220);
    line-height: 24px;
    opacity: 1
    transform: translate3d(0, 0, 0)
    .inner
        display: inline-block
        line-height: 24px
        font-size: 24px
        color: rgb(0, 160, 220)
        transition: all 0.4s linear
        transform: rotate(0)
    &.move-enter-active, &.move-leave-active
        transition: all .4s linear
    
    &.move-enter, &.move-leave-active
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
  .text 
    width: 24px;
    height: 24px;
    text-align: center;
    font-size: 10px;
    color: rgb(147, 153, 159);
    line-height: 24px;
    display: inline-block;
  

</style>
