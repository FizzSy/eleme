<template>
<div>
<div class="shopcart">
	<div class="content" @click='toggleList'>
		<div class="content-left">
			<div class="logo-wrapper">
				<div class="logo" :class='{"highlight":totalCount>0}'>
					<span class="icon-shopping_cart"></span>
				</div>
				<div class="num" v-show='totalCount>0'>{{totalCount}}</div>
			</div>
			<div class="price" v-show='totalCount>0' :class='{"highlight":totalCount>0}'>￥{{totalPrice}}元</div>
			<div class="price" v-show='!totalCount' :class='{"highlight":totalCount>0}'>未选购商品</div>
			<div class="desc">另需配送费 {{deliveryPrice}} 元</div>
		</div>
		<div class="content-right" @click.stop.prevent='pay'>
			<div class="pay" :class='{"payfor":totalPrice>=minPrice}'>
				{{payDesc}}
			</div>
		</div>
	</div>
	<transition name='stretch'>
		<div class="shopcart-list" v-show='listShow'>
			<div class="list-header">
				<h2 class="title">购物车</h2>
				<span class="empty" @click='empty'>清空</span>
			</div>
			<div class="list-content" ref='listContent'>
				<ul>
					<li class="food" v-for='food in selectFoods'>
						<span class="name">{{food.name}}</span>
						<div class="price">
							<span>￥{{food.price*food.count}}</span>
						</div>
						<div class="cartbtn-wrapper">
							<cartbtn :food='food'></cartbtn>
						</div>
					</li>
				</ul>
			</div>
		</div>				
	</transition>
</div>
<transition name='mask'>
	<div class="list-mask" @click='hideList' v-show='listShow'></div>				
</transition>	
</div>		
</template>

<script>
import BScorll from 'better-scroll';
import cartbtn from '../cartbtn/cartbtn.vue';
export default {
  props:{
	selectFoods:{
		type:Array,
		default(){
			return [
				{

				}
			]
		}
	},
	deliveryPrice:{
		type:Number,
		default:0
	},
	minPrice:{
		type:Number,
		default:0
	},
  },
  computed:{
  	listShow(){
  		if(!this.totalCount){
  			this.fold = true;
  		}
  		let show = !this.fold;
  		if(show){
  			this.$nextTick(() => {
  				if(!this.scroll){
					this.scroll = new BScorll(this.$refs.listContent,{
						click:true
					});  					
  				}
			this.scroll.refresh();
			});   
  		}
  		return show;
  	},
  	totalPrice(){
  		let totalPrice = 0;
  		this.selectFoods.forEach((food) => {
  			if(food.price>0)
  			totalPrice += food.price * food.count;

  		})
  		return totalPrice;
  	},
  	totalCount(){
  		let totalCount = 0;
  		this.selectFoods.forEach((food) => {
  			if(food.count>0){
  				totalCount += food.count;
  			}
  		})
  		return totalCount;  		
  	},
  	payDesc(){
  		if(this.totalPrice === 0){
  			return `￥${this.minPrice}元起送`;
  		}else if(this.totalPrice<this.minPrice){
  			return `还差￥${this.minPrice-this.totalPrice}元起送`
  		}else{
  			return '去结算'
  		}
  	}
  },
  methods:{
  	toggleList(){
  		if(!this.totalCount){
  			return;
  		}
  		this.fold = !this.fold;
  	},
  	empty(){
  		this.selectFoods.forEach((food) => {
  			food.count = 0;
  		})
  	},
  	hideList(){
  		this.fold = true;
  	},
  	pay(){
  		if(this.totalPrice<this.minPrice){
  			return;
  		}
  		window.alert(`支付${this.totalPrice}元`);
  	}  	
  },
  data(){
  	return{
  		fold:true
  	}
  },
  created(){
  },
  components:{
  	cartbtn
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
    .shopcart{
    	position: fixed;
    	left: 0;
    	bottom: 0;
    	z-index:50;
    	width: 100%;
    	height: 48px;
    	background: black;
    	.content{
    		display: flex;
    		background: #141d27;
    		font-size:0;
    		.content-left{
    			flex:1;
    			.logo-wrapper{
					display: inline-block;
					position: relative;
					top:-10px;
					margin:0 12px;
					padding: 6px;
					width: 56px;
					height: 56px;
					box-sizing: border-box;
					vertical-align: top;
					border-radius: 50%;
					.logo{
						width: 100%;
						height: 100%;
						border-radius: 50%;
						text-align: center;
						background:#2b343c;
						.icon-shopping_cart{
							line-height: 44px;
							font-size: 24px;
							color: #80858a;
						}
						&.highlight{
							background: rgb(0,160,220);
							.icon-shopping_cart{
								color: #fff;
							}							
						}
					}
					.num{
						position: absolute;
						top: 0;
						right: 0;
						width: 24px;
						height: 16px;
						line-height: 16px;
						text-align: center;
						border-radius: 16px;
						font-size: 10px;
						font-weight: 700;
						color:#fff;
						background: red;
						box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
					}
    			}
    			.price{
					display: inline-block;
					vertical-align: top;
					line-height: 24px;
					margin-top:12px;
					padding: 0 12px;
					box-sizing: border-box; 
					border:1px solid rgba(255,255,255,0.1);
					font-size: 10px;
					font-weight: 700;
					color:rgba(255,255,255,0.4);
					&.highlight{
						color:#fff;
					}
    			}
    			.desc{
					display: inline-block;
					vertical-align: top;
					line-height: 24px;
					margin:12px 0 0 12px;
					font-size: 10px;
					color:rgba(255,255,255,0.4);
    			}
    		}
    		.content-right{
    			flex:0 0 105px;
    			width: 105px;
    			.pay{
    				height: 48px;
    				line-height: 48px;
    				text-align: center;
    				color:rgba(255,255,255,0.4);
    				font-size: 12px;
    				font-weight: 700;
    				background: #2b333b;
    				&.payfor{
    					background: #00b43c;
    					color: #fff;
    				}
    			}
    		}
    	}
		.shopcart-list{
			position: absolute;
			top:0;
			left:0;
			z-index: -1;
			width: 100%;
			transform: translate3d(0, -100%, 0);
	      &.stretch-enter-active,&.stretch-leave-active{
	        transition: all 0.5s linear;
	      }
	      &.stretch-enter,&.stretch-leave-active{  
	        transform: translate3d(0,0,0);
	      }   	
	      .list-header{
	      	height: 40px;
	      	line-height: 40px;
	      	padding: 0 18px;
	      	background: #f3f5f7;
	      	.title{
	      		float: left;
	      		font-size: 14px;
	      		color:rgb(7,17,27);
	      	}
	      	.empty{
	      		float: right;
	      		font-size: 12px;
	      		color: rgb(0,160,220);
	      	}
	      }	
	      .list-content{
	      	padding: 0 18px;
	      	max-height: 217px;
	      	overflow: hidden;
	      	background: #fff;
	      	.food{
				position: relative;
				padding: 12px 0;
				box-sizing: border-box;
				border-bottom:1px solid rgba(7,17,27,.1);
				.name{
					line-height: 24px;
					font-size: 14px;
					color: rgb(7,17,27);
				}
				.price{
					position: absolute;
					right:90px;
					bottom: 12px;
					line-height: 24px;
					color: red;
					font-weight: 700;
					font-size: 14px;
				}
				.cartbtn-wrapper{
					position: absolute;
					right:0;
					bottom:6px;
				}
	      	}
	      }		
		}	
    }
	.list-mask{
		position: fixed;
		left:0;
		top:0;
		width: 100%;
		height: 100%;
		z-index:40;
		opacity: 1;
		backgrop-filter:blur(10px);
		background: rgba(7,17,27,0.6);
      &.mask-enter-active,&.mask-leave-active{
        transition: all 0.5s linear;
      }
      &.mask-enter,&.mask-leave-active{  
        opacity:0;
        background: rgba(7,17,27,0);
      }   		
	}        
</style>
