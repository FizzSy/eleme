<template>
	<div class="star" :class='starType'>
		<span v-for='item in itemClasses' :class='item' class="star-item"></span>
	</div>
</template>

<script>
const length = 5;
export default {
	props:{
		score:{
			type:Number
		},
		size:{
			type:Number
		}
	},
	computed:{
		starType(){
			return 'star-'+this.size
		},
		itemClasses(){
			let result = [];
			let score = Math.floor(this.score*2) / 2;
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
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
	.star{
		margin-top: 18px;
		padding: 2px 0; 
		font-size: 0;
		text-align: center;
		&.star-48{
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
		&.star-36{
			.star-item{
				display: inline-block;
				width: 15px;
				height: 15px;
				margin-right: 6px;
				background-repeat: no-repeat; 
				background-size: 100%;			
				&:last-child{margin-right:0;}
				&.on{
					background-image: url('../../assets/imgs/star36_on@2x.png');
				}
				&.half{
					background-image: url('../../assets/imgs/star36_half@2x.png');
				}
				&.off{
					background-image: url('../../assets/imgs/star36_off@2x.png');
				}				
			}
		}
		&.star-24{
			.star-item{
				display: inline-block;
				width: 10px;
				height: 10px;
				margin-right: 3px;
				background-repeat: no-repeat; 
				background-size: 100%;			
				&:last-child{margin-right:0;}
				&.on{
					background-image: url('../../assets/imgs/star24_on@2x.png');
				}
				&.half{
					background-image: url('../../assets/imgs/star24_half@2x.png');
				}
				&.off{
					background-image: url('../../assets/imgs/star24_off@2x.png');
				}				
			}
		}				
	}			
</style>
