<template>
  <div class="order">
    <van-nav-bar title="我的订单" />
    <van-tabs v-model="active" @click="tabClickHandler" color="#1659a0">
      <van-tab title="全部">
      </van-tab>
      <van-tab title="待派单">
      </van-tab>
      <van-tab title="待接单">
      </van-tab> 
      <van-tab title="待服务">
      </van-tab>
      <van-tab title="待确认">
      </van-tab>
      <van-tab title="已完成">
      </van-tab>
    </van-tabs>
    <div>
        <briup-order-item v-for="o in orders" 
        :key="o.id" 
        :data="o"></briup-order-item>
    </div>
  </div>
</template>
<script>
import { mapActions, mapState ,mapGetters} from 'vuex'
import axios, { get } from '../../http/axios'
export default {
  data(){
    return {
      active:0,
      orders:[],
    }
  },
  computed:{
    ...mapState("user",["info"]),
  },
  methods:{
    //点击tab的时候执行的函数
    tabClickHandler(name,title){
      this.status = tltle=="全部"?null:tltle;
      this.loadOrders();
      //this.$toast(name+":"+tltle);
    },
    loadOrders(){
      let url = "/order/query?";
      let params = {
        customerId:this.info.id,
        status:null,
      };
      get(url).then((response)=>{
        this.orders =response.data;
      });
    }
  },
  created(){
    this.loadOrders();
  }
}
</script>
<style scoped>
.order {
  background: #f1f1f1;
}
</style>