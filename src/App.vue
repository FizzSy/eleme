<template>
  <div>
    <v-header :seller='seller'></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller='seller'></router-view>
    </keep-alive>
  </div>
</template>

<script>
import header from './components/header/header.vue'
import {urlParse} from './assets/js/util.js'
export default {
  data(){
    return{
      seller:{
          id: (() => {
            let queryParam = urlParse();
            console.log(queryParam)
          })()
      }
    }
  },
  created(){
    this.$http.get('http://localhost:8080/api/seller?id='+this.seller.id).then(info => {
        this.seller = Object.assign({},this.seller,info.data.data);
    })
  },
  components:{
    'v-header':header
  }
}
</script>

<style lang='less'>
.tab{
  display: flex;
  width:100%;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid rgba(7,17,27,.1);
  .tab-item{
    flex:1;
    text-align: center;
    a{
      display: block;
      font-size: 14px;
      color: rgb(77,85,93);
      &.active{color:rgb(240,20,20);}
    }
  }
}  
</style>
