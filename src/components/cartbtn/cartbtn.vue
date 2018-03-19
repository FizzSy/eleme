<template>
  <div class="cart-btn">
    <transition name='move'>
      <div class="cart-decrease" @click.stop.prevent='decreaseCart' v-show='food.count>0'>
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show='food.count>0'>{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent='addCart'></div>
  </div>
</template>

<script>
import Vue from 'vue';
export default {
  props:{
    food:{
      type:Object
    }
  },
  data(){
    return{

    } 
  },
  computed:{

  },
  methods:{
    addCart(){
      if (!event._constructed) { //阻止非Vue事件
          return;
        }
      if(!this.food.count){
        Vue.set(this.food,'count',1)
      }else{
        this.food.count ++;
      }
    },
    decreaseCart(){
      if (!event._constructed) { //阻止非Vue事件
          return;
        }
      if(this.food.count>0){
        this.food.count --;
      }
    }
  },
  created(){

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
  .cart-btn{
    font-size: 0;
    .cart-count{
      display: inline-block;
      vertical-align: top;
      width: 12px;
      padding-top: 6px;
      line-height: 24px;
      text-align: center;
      font-size: 10px;
      color:rgb(147,153,159);
    }
    .cart-decrease{
      display: inline-block;
      padding: 6px;
      opacity: 1;
      transform: translate3d(0,0,0);
      .inner{
        display: inline-block;
        font-size: 24px;
        line-height: 24px;
        color:rgb(0,160,220); 
        transform: rotate(0);   
        transition: all 0.4s linear;
      }
      &.move-enter-active,&.move-leave-active{
        transition: all 0.4s linear;
      }
      &.move-enter,&.move-leave-active{
        opacity: 0;   
        transform: translate3d(24px, 0, 0);
        .inner{
          transform: rotate(180deg);
        }
      }     
    }
    .cart-add{
      display: inline-block;
      padding: 6px;
      font-size: 24px;
      line-height: 24px;
      color:rgb(0,160,220);
    }             
  }
</style>
