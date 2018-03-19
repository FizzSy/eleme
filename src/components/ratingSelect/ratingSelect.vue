<template>
  <div class="rating-select">
  	<div class="rating-type">
  		<span @click='select(2,$event)' class="block positive" :class='{"active":selectType === 2}'>
  			{{desc.all}}
  			<span class="count">({{ratings.length}})</span>
  		</span>
  		<span @click='select(0,$event)' class="block positive" :class='{"active":selectType === 0}'>
  			{{desc.positive}}
  			<span class="count">({{positive.length}})</span>
  		</span>
  		<span @click='select(1,$event)' class="block negative" :class='{"active":selectType === 1}'>
  			{{desc.negative}}
  			<span class="count">({{negative.length}})</span>
  		</span>
  	</div>
  	<div class="switch" :class='{"on":onlyContent===true}'>
  		<span class="icon-check_circle" @click='toggleContent($event)'></span>
  		<span class="text">只看有内容的评价</span>
  	</div>
  </div>
</template>

<script>
const positive = 0;
const negative = 1;
const all = 2;
export default {
  props:{
  	ratings:{
  		type:Array,
  		default(){
  			return [];
  		}
  	},
  	selectType:{
  		type:Number,
  		default:all
  	},
  	onlyContent:{
  		type:Boolean,
  		default:true
  	},
  	desc:{
  		type:Object,
  		default(){
  			return {
  				all : '全部',
  				positive : '满意',
  				negative : '不满意'
  			}
  		}
  	}
  },
  computed:{
	positive(){
		return this.ratings.filter((rating) => {
			return rating.rateType === positive;
		})
	},
	negative(){
		return this.ratings.filter((rating) => {
			return rating.rateType === negative;
		})
	}	
  },
  methods:{
  	toggleContent(event){
  		if(!event._constructed){
  			return ;
  		}
  		this.$emit('toggle');
  	},
  	select(selectType,event){
  		if(!event._constructed){
  			return ;
  		}
  		this.$emit('select',selectType);    //子组件传递数值改变父组件的data
  	}
  },
  data(){
  	return{
  		
  	}
  },
  created(){

  },
  components:{
  	
  }  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>
	.rating-select{
		.rating-type{
			padding: 18px 0;
			margin: 0 18px;
			font-size: 0;
			border-bottom: 1px solid rgba(7,17,27,.1);
			.block{
				display: inline-block;
				line-height: 16px; 
				padding: 8px 12px;
				margin-right: 8px;
				border-radius: 2px;
				font-size: 12px;
				color: rgb(77,85,93);
				.count{
					margin-left:8px;
					font-size: 8px;
				}			
				&.positive{
					background: rgba(0,160,220,0.2);
					&.active{
						background: rgb(0,160,220,0);
					}						
				}
				&.negative{
					background: rgba(77,85,93,0.2);
					&.active{
						background: rgb(77,85,93);
					}						
				}	
				&.active{
					color: #fff;
				}			
			}			
		}
		.switch{
			padding: 12px 18px;
			line-height: 24px;
			border-bottom: 1px solid rgba(7,17,27,.1);
			color:rgb(147,153,159);
			font-size: 0;
			&.on{
				.icon-check_circle{
					color:#00c850;
				}
			}
			.icon-check_circle{
				margin-right: 4px;
				font-size: 24px;
			}
			.text{
				font-size: 12px;
				vertical-align: top;
			}
		}
	}
</style>
