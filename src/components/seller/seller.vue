<template>
  <div class="seller" ref='seller'>
  	<div class="seller-content">
  		<div class="overview">
  			<h2 class="title">{{seller.name}}</h2>
  			<div class="desc">
  				<star :size='36' :score='seller.score'></star>
  				<span class="text">({{seller.ratingCount}})</span>
  				<span class="text">月售{{seller.sellCount}}单</span>
  			</div>
  			<ul class="remark">
  				<li class="block">
  					<h2>起送价</h2>
  					<div class="content">
  						<span class="stress">{{seller.minPrice}}</span>元
  					</div>
  				</li>
  				<li class="block">
  					<h2>商家配送</h2>
  					<div class="content">
  						<span class="stress">{{seller.deliveryPrice}}</span>元
  					</div>
  				</li> 
  				<li class="block">
  					<h2>平均配送时间</h2>
  					<div class="content">
  						<span class="stress">{{seller.deliveryTime}}</span>分钟
  					</div>
  				</li>   				 				
  			</ul>
  			<div class="favorite" @click='toggleFavorite($event)'>
  				<span class="icon-favorite" :class='{"active":favorite}'></span>
  				<span class="text">{{favoriteText}}</span>
  			</div>
  		</div>
  		<split></split>
  		<div class="bulletin">
  			<h1 class="title">公告与活动</h1>
  			<div class="content-wrapper">
  				<p class="content">{{seller.bulletin}}</p>
  			</div>
			<ul v-if='seller.supports' class="supports">
				<li class="support-item" v-for='(item,i) in seller.supports'>
					<span class="icon" :class='classMap[seller.supports[i].type]'></span>
					<span class="text">{{seller.supports[i].description}}</span>
				</li>
			</ul>  			
  		</div>
		<split></split>	
		<div class="pics">
			<h1 class="title">商家实景</h1>
			<div class="pic-wrapper" ref='picWrapper'>
				<ul class="pic-list" ref='picList'>
					<li class="pic-item" v-for='pic in seller.pics'>
						<img :src="pic" width='120' height='90'>
					</li>
				</ul>
			</div>
		</div>
		<split></split>	
		<div class="info">
			<h1 class="title">商家信息</h1>
			<ul>
				<li class="info-item" v-for='info in seller.infos'>{{info}}</li>
			</ul>
		</div>
  	</div>
  </div>
</template>

<script>
import BScroll from 'better-scroll';
import star from '../../components/star/star.vue'
import split from '../../components/split/split.vue'
export default {
	props:{
		seller:{
			type:Object
		}
	},
  data () {
    return {
      favorite:false
    }
  },
  computed:{
  	favoriteText(){
  		return this.favorite?'已收藏':'收藏'
  	}
  },
  watch:{
  	'seller'(){
	    this.$nextTick(() => {
	        this.initScroll();
	        this.initPics();
	    })  
  	}
  },
  methods:{
  	toggleFavorite(event){
  		if(!event._constructed){
  			return ;
  		}
  		this.favorite = !this.favorite;
  	},
  	initScroll(){
  		if(!this.scroll){
  			this.scroll = new BScroll(this.$refs.seller,{click:true})
  		}else{
  			this.scroll.refresh();
  		}      		
  	},
  	initPics(){
  		let picWidth = 120;
  		let margin = 6;
  		let width = (picWidth+margin) * this.seller.pics.length-margin;
  		this.$refs.picList.style.width = width +'px';
		this.$nextTick(() => {
			if(!this.picScroll){
				this.picScroll = new BScroll(this.$refs.picWrapper, {
					scrollX:true,
					eventPassthrough:'vertical'
				})
			}else{
				this.scroll.refresh();
			}
		})   		
  	}
  },	
  created(){
  	this.classMap=['decrease','discount','guarantee','invoice','special']
  },
  mounted(){
    this.$nextTick(() => {
        this.initScroll();
        this.initPics();
    })  
  },
  components:{
  	star,
  	split
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
  .seller{
	position: absolute;
	top:174px;
	bottom: 0;
	width: 100%;
	left: 0;
	overflow: hidden;
	  .overview{
	  	position: relative;
	  	padding: 18px;
	  	.title{
	  		margin-bottom: 8px;
	  		line-height: 14px;
	  		color: rgb(7,17,27);
	  		font-size: 14px;
	  	}
	  	.desc{
	  		padding-bottom: 18px;
	  		border-bottom: 1px solid rgba(7,17,27,.1);
	  		font-size: 0;
	  		.star{
	  			margin-top: 0;
	  			display: inline-block;
	  			margin-right: 8px;
	  			vertical-align: top;
	  		}
	  		.text{
	  			display: inline-block;
	  			margin-right: 12px;
	  			line-height: 18px;
	  			font-size: 10px;
	  			color: rgb(77,85,93);
	  		}
	  	}
	  	.remark{
	  		display: flex;
	  		padding-top: 18px;
	  		.block{
	  			flex:1;
	  			text-align: center;
	  			border-right: 1px solid rgba(7,17,27,.1);
	  			&:last-child{
	  				border:none;
	  			}
	  			h2{
	  				margin-bottom: 4px;
	  				line-height: 10px;
	  				font-size: 10px;
	  				color:rgb(147,153,159);
	  			}
	  			.content{
	  				line-height: 24px;
	  				font-size: 10px;
	  				color: rgb(7,17,27);
	  				.stress{
	  					font-size: 24px;
	  				}
	  			}
	  		}
	  	}
	  	.favorite{
	  		position: absolute;
	  		width: 50px;
	  		right: 11px;
	  		top: 18px;
	  		text-align: center;
	  		.icon-favorite{
	  			display: block;
	  			margin-bottom: 4px;
	  			line-height: 24px;
	  			color:#d4d6d9;
	  			font-size: 24px;
	  			&.active{
	  				color: rgb(240,20,20);
	  			}
	  		}
	  		.text{
	  			line-height: 10px;
	  			font-size: 10px;
	  			color: rgb(77,85,93);
	  		}
	  	}
	  }  
	  .bulletin{
	  	padding: 18px 18px 0 18px;
	  	.title{
	  		margin-bottom: 8px;
	  		line-height: 14px;
	  		color: rgb(7,17,27);
	  		font-size: 14px;
	  	}
	  	.content-wrapper{
	  		padding: 0 12px 16px 12px;
	  		border-bottom: 1px solid rgba(7,17,27,.1);
	  		.content{
	  			line-height: 24px;
	  			font-size: 12px;
	  			color: rgb(240,20,20);
	  		}
	  	}
	  	.supports{
	  		.support-item{
				padding: 16px 12px;
				border-bottom: 1px solid rgba(7,17,27,.1);
				font-size: 0;
				&:last-child{
					border:none;
				}
	  		}
	  		.text{
	  			line-height: 16px;
	  			font-size: 12px;
	  			color: rgb(7,17,27);
	  		}
	  	}
	  }	
	  .pics{
	  	padding: 18px;
	  	.title{
	  		margin-bottom: 12px;
	  		line-height: 14px;
	  		color: rgb(7,17,27);
	  	}
	  	.pic-wrapper{
	  		width: 100%;
	  		overflow: hidden;
	  		white-space: nowrap;
	  		.pic-list{
	  			font-size: 0;
	  			.pic-item{
	  				display: inline-block;
	  				margin-right: 6px;
	  				width: 120px;
	  				height: 90px;
	  				&:last-child{
	  					margin:0;
	  				}
	  			}
	  		}
	  	}
	}
  	.info{
  		padding: 18px 18px 0 18px;
  		color:rgb(7,17,27);
  		.title{
  			padding-bottom: 12px;
  			line-height: 14px;
  			border-bottom: 1px solid rgba(7,17,27,.1);
  			font-size: 14px;
  			.info-item{
  				padding: 16px 12px;
  				line-height: 16px;
  				border-bottom: 1px solid rgba(7,17,27,.1);
  				font-size: 12px;
  				&:last-child{
  					border:none;
  				}
  			}
  		}
  	}	
  }
.icon{
	display: inline-block;
	vertical-align: top;
	width: 16px;
	height: 16px;
	margin-right: 6px;
	background-size:100%;
	background-repeat: no-repeat;			
	&.decrease{
		background-image: url(../../assets/imgs/decrease_4@2x.png);
	}
	&.discount{
		background-image: url(../../assets/imgs/discount_4@2x.png);
	}
	&.guarantee{
		background-image: url(../../assets/imgs/guarantee_4@2x.png);
	}
	&.invoice{
		background-image: url(../../assets/imgs/invoice_4@2x.png);
	}
	&.special{
		background-image: url(../../assets/imgs/special_4@2x.png);
	}			
}		  
</style>
