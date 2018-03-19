<template>
	<div>
  	  <div class="goods">
	    <div class="menu-wrapper" ref='menuWrapper'>
	      <ul>
	        <li @click=selectItem(i) v-for='(item,i) in goods' class="menu-item" :class="{'currentIndex':active == i}" ref='menuList'>
	          <div class="text">
	            <span v-show='item.type>0' class="icon" :class='classMap[item.type]'></span>
	            {{item.name}}
	          </div>
	        </li>
	      </ul>
	    </div>
	    <div class="foods-wrapper" ref='foodsWrapper'>
	      <ul>
	        <li v-for='item in goods' class="food-list" ref="foodList">
	          <h1 class="title">{{item.name}}</h1>
	          <ul class="food-ul">
	            <li v-for='food in item.foods' @click='selectFood(food,$event)' class="food-item">
	              <div class="food-img">
	                <img :src="food.icon" width="57" height='57'>
	              </div>
	              <div class="content">
	                <h2 class="name">{{food.name}}</h2>
	                <h2 class="desc">{{food.description}}</h2>
	                <div class="extra">
	                  <span class="count">月售{{food.sellCount}}份</span>
	                  <span>好评率{{food.rating}}%</span>
	                </div>
	                <div class="price">
	                  <span class="new">￥{{food.price}}</span>
	                  <span class="old" v-show='food.oldPrice'>{{food.oldPrice}}</span>
	                </div>
	                <div class="cart-wrapper">
	                	<cartbtn :food='food'></cartbtn>
	                </div>
	              </div>
	            </li>
	          </ul>
	        </li>
	      </ul>
	    </div>    
	    <shopcart :selectFoods='selectFoods' :deliveryPrice='seller.deliveryPrice' :minPrice='seller.minPrice'></shopcart>
	  </div>	
	  <food :food='selectedFood' ref='food'></food>	
	</div>
</template>

<script>
import BScorll from 'better-scroll';
import shopcart from '../shopcart/shopcart.vue';
import cartbtn from '../cartbtn/cartbtn.vue';
import food from '../food/food.vue';
export default {
  props:{
    seller:{
      type:Object
    }
  },
  data(){
    return{
      goods:[],
      listHeight:[],
      scrollY:0,
      length:0,
      active:0,
      selectedFood:{}
    }
  },
  computed:{
    currentIndex(){
      for(let i = 0; i < this.listHeight.length;i++){
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i+1];
        if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
          this.followScroll(i);
          console.log(i);
          return i;
        }
      }
      return 0;
    },
    selectFoods(){
    	let foods = [];
    	this.goods.forEach((good) => {
    		good.foods.forEach((food) => {
  				if(food.count>0){
  					foods.push(food);
  				}
    		})
    	})
    	return foods;
    }
  },
  methods:{
  	selectFood(food,event){
  		if(!event._constructed){
  			return;
  		}
		this.selectedFood = food;
		this.$refs.food.show();  //调用子组件的方法 
  	},  	
    initScroll(){
      this.menuScroll = new BScorll(this.$refs.menuWrapper,{click:true});
      this.foodsScroll = new BScorll(this.$refs.foodsWrapper,{click:true,probeType:3});
      this.foodsScroll.on('scroll',(pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
    caculateHeight(){
      let foodList = this.$refs.foodList; 	//此处有问题 取不到
      let height = 0;
      this.listHeight.push(height);
      for(let i =0;i<foodList.length;i++){
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    },
    selectItem(i){
    	if (!event._constructed) { //针对PC端的设置,阻止非Vue事件
          return;
        }
    	let foodList = this.$refs.foodList;
    	let el =foodList[i];
    	this.foodsScroll.scrollToElement(el,300);
    	this.active = i;
    },
    followScroll(i){
      let menuList = this.$refs.menuList;
      let el = menuList[i];
      console.log(666);
      this.menuScroll.scrollToElement(el,300,0,-100);
    }
  },
  created(){
    this.$http.get('http://localhost:8080/api/goods').then(info => {
        this.goods = info.data.data;
        this.length = this.goods.length;
    		this.$nextTick(() => {
    			this.initScroll();
          this.caculateHeight();
    		});        
    })
    this.classMap=['decrease','discount','guarantee','invoice','special'];
  },
  components:{
  	shopcart,
  	cartbtn,
  	food
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
  .goods{
    display: flex;
    position: absolute;
    top:174px;
    bottom:46px;
    width: 100%;
    overflow: hidden;
    .menu-wrapper{
      flex:0 0 80px;
      width: 80px;
      background: #f3f5f7;
      text-align: center;
      .menu-item{
        display: table;
        height:54px;
        width: 56px;
        line-height: 14px;
        padding: 0 12px;
        border-bottom:1px solid rgba(7,17,27,.1);
        &:last-child{
          border-bottom:0;
        } 
        &.currentIndex{
          position: relative;
          z-index: 10;
          background: #fff;
          font-weight: 700;
          margin-top: -1px;
          border-bottom: 0;
        }     
        .text{
          display: table-cell;
          height:56px;
          width: 56px;
          vertical-align: middle;
          box-sizing: border-box;
          font-size: 12px;
        }
      }
    }
    .foods-wrapper{
      flex:1;
      .title{
        padding-left: 14px;
        height: 26px;
        line-height: 26px;
        border-left: 2px solid #d9dde1;
        font-size: 12px;
        color: rgb(147,153,159);
        background: #f3f5f7;
      }
      .food-item{
        display: flex;
        margin: 18px 18px 0px 18px;
        padding-bottom: 18px;
        border-bottom: 1px solid rgba(7,17,27,.1);
        &:last-child{
          border:0;
          margin-bottom: 0;
        }
        .food-img{
          flex:0 0 57px;
          margin-right: 10px;
        }
        .content{
          position: relative;
          flex:1;
          .name{
            margin: 2px 0 8px 0;
            height: 14px;
            line-height: 14px;
            font-size: 14px;
            color: rgb(7,17,27);
          }
          .desc,.extra{
            line-height: 14px;
            font-size:10px;
            color:rgb(147,153,159);
          }
          .desc{
            margin-bottom: 8px;
          }
          .count{
            margin-right: 12px;
          }
          .price{
            font-weight: 700;
            line-height: 24px;
            .new{
              margin-right: 18px;
              font-size: 14px;
              font-weight: 700;
              color:rgb(240,20,20);
            }
            .old{
              text-decoration: line-through;
              font-size: 10px;
              color:rgb(147,153,159);
            }
          }
          .cart-wrapper{
			position: absolute;
			right: 0;
			bottom: -8px;
          }
        }
      }
    }
  }
  .icon{
    display: inline-block;
    vertical-align: top;
    width: 12px;
    height: 12px;
    background-size:100%;
    background-repeat: no-repeat;   
    &.decrease{
      background-image: url(../../assets/imgs/decrease_3@2x.png);
    }
    &.discount{
      background-image: url(../../assets/imgs/discount_3@2x.png);
    }
    &.guarantee{
      background-image: url(../../assets/imgs/guarantee_3@2x.png);
    }
    &.invoice{
      background-image: url(../../assets/imgs/invoice_3@2x.png);
    }
    &.special{
      background-image: url(../../assets/imgs/special_3@2x.png);
    }     
  }   
</style>
