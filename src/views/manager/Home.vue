<template>
  <div class="home">
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
    <!-- 宫格 -->
    <!-- {{categories}} -->
    <!-- {{products}} -->
   <van-grid :column-num="3">
  <van-grid-item
    v-for="value in categories"
    :key="value.id"
    :icon="value.icon"
    :text="value.name"
  />
</van-grid>
<van-grid :column-num="1" icon-size="350px">
  <van-grid-item
    v-for="value in products"
    :key="value.id"
    :icon="value.photo"
    :text="value.name"
  />
</van-grid>
  </div>
</template>
<script>
import {mapState, mapActions} from 'vuex'
import {get,post} from '../../http/axios';
export default {
  data(){
    return{
      categories:[],
      products:[],
    };
  },
  created(){
    //查询栏目信息
    this.loadCategories();
    //查询产品信息
    this.loadProducts();
  },
  methods:{
    //加载栏目信息
    loadCategories(){
      let url = "/category/findAll";
      get(url).then((response)=>{
        this.categories = response.data.slice(0,6);
      })
    },
    //加载产品信息
    loadProducts(){
      let url = "/product/query";
      let params={
        page:0,
        pageSize:10,
      };
      post(url,params).then((response)=>{
        this.products = response.data.list;
      });
    },
  }
}
</script>
<style scoped>
.home {
  padding-bottom: 50px;
}
.header {
  height: 300px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>