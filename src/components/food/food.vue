<template>
  <transition name='move'>
  	  <div class="food" v-show='showFlag' ref='food'>
		<div class="food-content">
			<div class="image-header">
				<img :src="food.image" alt="">
			</div>
			<div class="content">
				<h2 class="title">{{food.name}}</h2>
				<div class="detail">
					<span class="sell-count">月售{{food.sellCount}}</span>
					<span class="rating">好评率{{food.rating}}%</span>
				</div>
                <div class="price">
                  <span class="new">￥{{food.price}}</span>
                  <span class="old" v-show='food.oldPrice'>{{food.oldPrice}}</span>
                </div>
				<div class="cartbtn-wrapper">
					<cartbtn :food='food'></cartbtn>
				</div>
				<transition name='fade'>
					<div class="buy" @click='addFood($event)' v-show='!food.count||food.count===0'>加入购物车</div>
				</transition>
			</div>
			<split v-show='food.info'></split>
			<div class="info" v-show='food.info'>
				<h2 class="title">商品信息</h2>
				<p class="text">{{food.info}}</p>
			</div>
			<split></split>
			<div class="rating">
				<h1 class="title">商品评价</h1>
				<ratingSelect @select='selectRating' @toggle='toggleContent' :selectType='selectType' :onlyContent='onlyContent' :desc='desc' :ratings='food.ratings'></ratingSelect>
				<div class="rating-wrapper">
					<ul v-show='food.ratings && food.ratings.length'>
						<li v-show='needShow(rating.rateType,rating.text)' v-for='rating in food.ratings' class="rating-item">
							<div class="user">
								<span class="name">{{rating.username}}</span>
								<img class='avatar' :src="rating.avatar" width='12' height='12' alt="">
							</div>
							<div class="time">{{rating.rateTime | formatDate}}</div>
							<p class="text">
								<span :class='{"icon-thumb_up":rating.rateType===0,"icon-thumb_down":rating.rateType===1}'></span>
								{{rating.text}}
							</p>
						</li>
					</ul>
					<div class="no-rating" v-show='!food.ratings || !food.ratings.length'>暂无评价</div>
				</div>
			</div>			
		</div>
		<div class="back" @click='hide'>
			<i class="icon-arrow_lift"></i>
		</div>		
	  </div>	
  </transition>	
</template>

<script>
import Vue from 'vue';
import BScorll from 'better-scroll';
import cartbtn from '../cartbtn/cartbtn.vue';
import split from '../split/split.vue';
import {formatDate} from '../../assets/js/date.js'
import ratingSelect from '../ratingSelect/ratingSelect.vue';


const positive = 0;
const negative = 1;
const all = 2;
export default {
  props:{
  	food:{
  		type:Object
  	}
  },
  computed:{
		
  },
  methods:{
  	needShow(type,text){
		if(this.onlyContent && !text){
			return false;
		}
		if(this.selectType === all){
			return true;
		}else{
			return type === this.selectType;
		}
  	},
  	selectRating(type){
  		this.selectType = type;
  		this.$nextTick(() => {
  			this.scroll.refresh();
  		})
  	},
  	toggleContent(){
  		this.onlyContent = !this.onlyContent;
  		this.$nextTick(() => {
  			this.scroll.refresh();
  		})
  	},
  	show(){
  		this.showFlag = true;
  		this.selectType = all;
  		this.onlyContent = true;
  		this.$nextTick(() => {
  			if(!this.scroll){
  				this.scroll = new BScorll(this.$refs.food,{
  					click:true
  				})
  			}
  		})
  	},
  	hide(){
  		this.showFlag = false;
  	},
  	addFood(event){
  		if(!event._constructed){
  			return;
  		}
  		Vue.set(this.food,'count',1);
  	}
  },
  data(){
  	return{
  		showFlag:false,
  		selectType:all,
  		onlyContent:true,
  		desc:{
  			all:'全部',
  			positive:'推荐',
  			negative:'吐槽'
  		}
  	}
  },
  filters:{
  	formatDate(time){
  		let date = new Date(time);
  		return formatDate(date,'yyy-MM-dd hh:mm');
  	}
  },
  components:{
  	cartbtn,
  	ratingSelect,
  	split
  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
    .food{
    	position: fixed;
    	left: 0;
    	top:0;
    	bottom: 48px;
    	z-index: 30;
    	width: 100%;
    	background: #fff;
    	transform: translate3d(0, 0, 0);
      &.move-enter-active,&.move-leave-active{
        transition: all 0.4s linear;
      }
      &.move-enter,&.move-leave-active{  
        transform: translate3d(100%, 0, 0);
      }   
      .back{
      	position: fixed;
      	top:10px;
      	left: 0;
      	.icon-arrow_lift{
      		display: block;
      		padding: 10px;
      		font-size: 20px;
      	}
      }	         
      .image-header{
      	position: relative;
      	width: 100%;
      	height:0;
      	padding-top: 100%;  			/* 高度与宽度一致 */
      	img{
      		position: absolute;
      		left: 0;
      		top: 0;
      		width: 100%;
      		height: 100%;
      	}
      }
      .content{
      	position: relative;
      	padding: 18px;
      	.title{
      		line-height: 14px;
      		margin-bottom: 8px;
      		font-size: 14px;
      		font-weight: 700;
      		color: rgb(7,17,27);
      	}
      	.detail{
      		margin-bottom: 18px;
      		line-height: 10px;
      		font-size: 0;
      		.sell-count,.rating{
      			font-size: 10px;
      			color: rgb(147,153,159);
      		}
      		.sell-count{
      			margin-right: 12px;
      		}
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
	    .cartbtn-wrapper{
	    	position: absolute;
	    	right:12px;
	    	bottom: 12px;
	    }
    	.buy{
    		position: absolute;
    		right:18px;
    		bottom: 18px;
    		z-index:10;
    		height:24px;
    		line-height: 24px;
    		padding: 0 12px;
    		font-size: 10px;
    		color: #fff;
    		background: rgb(0,160,220);
    		border-radius:10px;
    		opacity: 1;
	      &.fade-enter-active,&.fade-leave-active{
	        transition: all 0.2s linear;
	      }
	      &.fade-enter,&.fade-leave-active{  
	        opacity:0;
	      }       		
    	}	        	
      }   
      .info{
      	padding: 18px;
      	.title{
			line-height: 14px;
			margin-bottom: 6px;
			font-size: 14px;
			color:rgb(7,17,27);
      	}
      	.text{
      		line-height: 24px;
      		font-size: 12px;
      		padding: 0 8px;
      		color:rgb(77,85,93);
      	}
      }
      .rating{
      	padding-top: 18px;
      	.title{
			line-height: 14px;
			margin-left: 18px;
			font-size: 14px;
			color:rgb(7,17,27);      		
      	}
      	.rating-wrapper{
      		padding: 0 18px;
      		.rating-item{
      			position: relative;
      			padding: 16px 0;
      			border-bottom: 1px solid rgba(7,17,27,.1);
      			.user{
      				position: absolute;
      				right:0;
      				top:16px;
      				line-height:12px;
      				font-size:0
      				.name{
      					display:inline-block;
      					margin-right: 6px;
						vertical-align: top;
      					font-size: 10px;
      					color:rgb(147,153,159);
      				}
      				.avatar{
      					border-radius: 50%;
      				}
      			}
      			.time{
      				margin-bottom: 6px;
      				line-height: 12px;
      				font-size: 10px;
      				color:rgb(147,153,159);
      			}
      			.text{
      				line-height: 16px;
      				font-size: 12px;
      				color:rgb(7,17,27);
      				.icon-thumb_up,.icon-thumb_down{
      					margin-right: 4px;
      					line-height: 16px;
      					font-size: 12px;
      				}
      				.icon-thumb_up{
      					color: rgb(0,160,220);
      				}
      				.icon-thumb_down{
      					color: rgb(147,153,159);
      				}      				
      			}
      		}
      		.no-rating{
      			padding: 16px 0;
      			font-size: 12px;
      			color:rgb(147,153,159);
      		}
      	}
      }
    } 
	.split{
		width: 100%;
		height: 16px;
		border-top:1px solid rgba(7,17,27,.1);
		border-bottom:1px solid rgba(7,17,27,.1);
		background: #f3f5f7;
	}    
</style>
