<template>
  <div class="home">
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
   <div>
<!-- 分类 -->
    <van-grid :column-num="3">
  <van-grid-item
    v-for="value in categories"
    :key="value.id"
    :icon="value.icon"
    :text="value.name"
  />
</van-grid>
  
   <briup-product-item 
   @click="toBuyHandler(p)"
   v-for="p in products"
   :key="p.id"
   :data="p">
   </briup-product-item>
  
<!-- /分类 -->

   </div>
  </div>
</template>
<script>

import {get, post}from '../../http/axios';
export default {
data(){
  return{
    categories:[],
    products:[]
  }
},
  created(){
    //查询产品
    this.loadProducts();
    //查询栏目信息
    this.loadCategories();
  },
  methods:{
    toBuyHandler(p){
      alert(JSON.stringify(p));
      //跳转到订单页面，并且携带数据p
      this.$router.push({
        path:"/manager/order_confirm",
        query:p
      })
    },
    loadCategories(){
      let url = "/category/findAll"
      get(url).then((response)=>{
        //将查询结果，数组中的前6个
      this.categories=response.data.slice(0,6);
      })
    },
    //加载产品信息
    loadProducts(){
      let url = "/product/query"
      let params ={
        page:0,
        pageSize:100
      }
      post(url,params).then((response)=>{
        this.products= response.data.list;
      })
    }

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