<template>
    <briup-fulllayout title="订单确认">
    <!-- {{$route.query}}<br>
    {{info}}<br>
    {{addresses}} -->
    <van-dropdown-menu>
        <van-dropdown-item v-model="addressId" :options="options" />
    </van-dropdown-menu>
    订单详情：
    <div style="padding:0 2em">
        <p>服务名称：{{$route.query.name}}</p>
        <p>服务价格：{{$route.query.price}}</p>
        <p>服务数量：1</p>
        <p>服务费用小计：{{$route.query.price*1}}</p>
    </div>
    <div style="position:fixed;bottom:0;width:100%">
        <van-button block @click="submitHandler" type="primary">提交订单</van-button>
    </div>
    </briup-fulllayout>
</template>
<script>
import {mapState} from 'vuex'
import {get,post_obj_array} from '../../../http/axios'
export default {
    data(){
        return{
            addresses:[],
            addressId:0,
            orderLines:[],
        }
    },
    created(){
        this.loadAddress();
        let orderLine ={
            number:1,
            price:this.$route.query.price,
            productId:this.$route.query.id,
        };
        this.orderLines.push(orderLine);
    },
    computed:{
        ...mapState("user",["info"]),
        options:function(){
            //将address的数据计算为一个新的数组返回
            return this.addresses.map(item=>{
                return{
                    text:item.province+" "
                    +item.city+" "
                    +item.area+" "
                    +item.address
                    ,value:item.id
                }
            })
        }
    },
    methods:{
        //提交订单
        submitHandler(){
            let url = '/order/save';
            let data = {
                customerId:this.info.id,
                addressId:this.addresses,
                orderLines:this.orderLines
            }
            post_obj_array(url,data).then((response)=>{
                this.$toast("提交成功");
                this.$router.push('/manager/order');
            })
        },
        loadAddress(){
            let id =this.info.id;
            let url ="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.addresses =response.data;
                //将查询到的第一个地址的id赋值给data中AddressId,表示默认地址
                this.addressId = this.addresses[0].id;
            })
        },
    },
}
</script>
<style scoped>
</style>