<template>
  <div class="header">
  	<div class="content-wrapper">
  		<div class="avatar">
  			<img :src="seller.avatar" width="64" height="64">
  		</div>
  		<div class="content">
  			<div class="title">
  				<span class="brand"></span>
  				<span class="name">{{seller.name}}</span>
  			</div>
	  		<div class="description">
	  			{{seller.description}}/{{seller.deliveryTime}}分钟送达
	  		</div>
	  		<div v-if='seller.supports' class="support">
	  			<span class="icon" :class='classMap[seller.supports[0].type]'></span>
	  			<span class="text">{{seller.supports[0].description}}</span>
	  		</div>  			
  		</div>
  		<div class="support-count" v-if='seller.supports'>
  			<span class="count" @click='detailShow=true'>{{seller.supports.length}}个</span>
  			<i class="icon-keyboard_arrow_right"></i>
  		</div>
  	</div>
  	<div class="bulletin-wrapper" @click='detailShow=true'>
  		<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
  		<i class="icon-keyboard_arrow_right"></i>
  	</div>
  	<div class="background">
  		<img :src='seller.avatar' width='100%' height='100%'>
  	</div>
	<transition name='fade'>
  		<div class="detail" v-show='detailShow'>
	  		<div class="detail-wrapper clearfix">
	  			<div class="detail-main">
	  				<h1 class="name">{{seller.name}}</h1>
	  				<star :size='48' :score='seller.score'></star>
	  				<div class="title">
	  					<div class="line"></div>
	  					<div class="text">优惠信息</div>
	  					<div class="line"></div>
	  				</div>
	  				<ul v-if='seller.supports' class="supports">
	  					<li class="support-item" v-for='(item,i) in seller.supports'>
	  						<span class="icon" :class='classMap[seller.supports[i].type]'></span>
							<span class="text">{{seller.supports[i].description}}</span>
	  					</li>
	  				</ul>
	  				<div class="title">
	  					<div class="line"></div>
	  					<div class="text">商家公告</div>
	  					<div class="line"></div>
	  				</div>  
	  				<div class="bulletin">
	  					<div class="content">{{seller.bulletin}}</div>
	  				</div>				
	  			</div>
	  		</div>
	  		<div class="detail-close">
	  			<i class="icon-close" @click='detailShow=false'></i>
	  		</div>
	  	</div>		
	</transition>
  </div>
</template>

<script>
import star from '../../components/star/star.vue'
const length = 5;
export default {
	props:{
		seller:{
			type:Object
		}
	},
	data(){
		return{
			detailShow:false
		}
	},
	computed:{
		itemClasses(){
			let result = [];
			let score = Math.floor(this.seller.score*2) / 2;
			let hasSpot = score % 1 !== 0; //判断有小数则用half半星
			let integer = Math.floor(score);
			for(let i = 0;i < integer;i ++){
				result.push('on');
			}
			if(hasSpot){
				result.push('half');
			}
			for(let i = result.length;i < length;i++){
				result.push('off');
			}
			return result;
		}
	},
	created(){
		this.classMap=['decrease','discount','guarantee','invoice','special']
	},
	components:{
		star
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
	.header{
		position: relative;
		color:#fff;
		background: rgba(7,17,27,.5);
		overflow: hidden;
		.content-wrapper{
			padding: 24px 12px 18px 24px;
			font-size: 0;
			position: relative;
			.support-count{
				position: absolute;
				right:12px;
				bottom: 18px;
				padding: 0 8px;
				height: 24px;
				line-height: 24px;
				border-radius: 14px;
				background:rgba(0,0,0,.2);
				text-align: center;
				.count{
					font-size: 10px;
					vertical-align: top;
				}
				.icon-keyboard_arrow_right{
					line-height: 24px;
					margin-left: 2px;
					font-size: 10px;
				}	
			}
			.avatar{
				display: inline-block;
				img{
					border-radius: 2px;
				}
			}
		}
		.bulletin-wrapper{
			position: relative;
			height: 28px;
			line-height: 28px;
			padding: 0 22px 0 12px;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			background:rgba(7,17,27,.2);
			.bulletin-title{
				display: inline-block;
				width: 22px;
				height: 12px;
				background: url('../../assets/imgs/bulletin@2x.png') no-repeat;
				background-size: 100%;
			}
			.bulletin-text{
				vertical-align: top;
				margin: 0 4px;
				font-size: 10px;
			}
			.icon-keyboard_arrow_right{
				position: absolute;
				font-size: 10px;
				right:12px;
				top:8px;
			}
		}
		.background{
			position: absolute;
			left: 0;
			top: 0;
			width: 100%;
			height: 100%;
			z-index:-1;
			filter:blur(10px);
		}
	}
	.detail{
		position: fixed;
		z-index: 100;
		top:0;
		left:0;
		width: 100%;
		height: 100%;
		overflow: auto;
		opacity: 1;
		background: rgba(7,17,27,0.8);	
		backdrop-filter:blur(10px);
		&.fade-enter-active,&.fade-leave-active{
			transition:all .5s linear;
		}
		&.fade-enter,&.fade-leave-to{
			opacity: 0;
			background: rgba(7,17,27,0);			
		}
		.detail-wrapper{
			width: 100%;
			min-height: 100%;
			.detail-main{
				margin-top: 64px;
				padding-bottom: 64px;
				.name{
					line-height: 16px;
					text-align: center;
					font-size: 16px;
					font-weight: 700;
				}
				.title{
					display: flex;
					width: 80%;
					margin: 28px auto 24px;
					.line{
						flex:1;
						position: relative;
						top:-6px;
						border-bottom: 1px solid rgba(255,255,255,0.2);
					}
					.text{
						padding: 0 12px;
						font-size: 14px;
						font-weight: 700;
					}
				}
				.supports{
					width: 80%;
					margin:0 auto;
					.support-item{
						padding: 0 12px;
						font-size: 0;
						&:not(:last-child){
							margin-bottom: 12px;
						}
						.icon{
							display: inline-block;
							width: 16px;
							height: 16px;
							vertical-align: top;
							margin-right: 6px;
							background-size: 100%;
							background-repeat:no-repeat; 
						}
						.text{
							line-height: 16px;
							font-size: 12px;
						}
					}						
				}
				.bulletin{
					width: 80%;
					margin: 0 auto;
					.content{
						padding: 0 12px;
						line-height: 24px;
						font-size: 12px;
					}
				}				
			}
		}
		.detail-close{			/* sticky-footer 布局 */
			position: relative;
			width: 32px;
			height: 32px;
			margin: -64px auto;
			clear: both;
			.icon-close{
				font-size: 32px;
			}
		}		
	}
	.star{
		margin-top: 18px;
		padding: 2px 0; 
		font-size: 0;
		text-align: center;
		.star-item{
			display: inline-block;
			width: 20px;
			height: 20px;
			margin-right: 22px;
			background-repeat: no-repeat; 
			background-size: 100%;			
			&:last-child{margin-right:0;}
			&.on{
				background-image: url('../../assets/imgs/star48_on@2x.png');
			}
			&.half{
				background-image: url('../../assets/imgs/star48_half@2x.png');
			}
			&.off{
				background-image: url('../../assets/imgs/star48_off@2x.png');
			}				
		}
	}		
	.content{
		display: inline-block;
		vertical-align: top;
		margin-left: 16px;
		font-size: 14px;
		.title{
			margin: 2px 0 8px 0;
			.brand{
				display: inline-block;
				width: 30px;
				height: 18px;
				background: url('../../assets/imgs/brand@2x.png') no-repeat;
				background-size: 100%;
				vertical-align: top;
			}
			.name{
				margin-left: 6px;
				font-size: 16px;
				line-height: 18px;
				font-weight: bold;
			}
		}
		.description{
			margin-bottom: 10px;
			line-height: 12px;
			font-size: 12px;
		}
		.support{
			.text{
				line-height: 12px;
				font-size: 10px;
			}
		}
	}
	.icon{
		display: inline-block;
		vertical-align: middle;
		width: 12px;
		height: 12px;
		margin-right: 4px;
		background-size:100%;
		background-repeat: no-repeat;			
		&.decrease{
			background-image: url(../../assets/imgs/decrease_1@2x.png);
		}
		&.discount{
			background-image: url(../../assets/imgs/discount_1@2x.png);
		}
		&.guarantee{
			background-image: url(../../assets/imgs/guarantee_1@2x.png);
		}
		&.invoice{
			background-image: url(../../assets/imgs/invoice_1@2x.png);
		}
		&.special{
			background-image: url(../../assets/imgs/special_1@2x.png);
		}			
	}		
</style>
