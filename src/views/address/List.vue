<template>
    <briup-fulllayout title="常用地址">
    <!-- {{addresses}} -->
    <!-- {{info}} -->
    <h3>常用地址：</h3>
    <van-list>
        <van-cell
            v-for="item in addresses"
            :key="item.id"
            :title="item.province+' '+item.city+' '+item.area+' '+item.address"
        ></van-cell>
    </van-list>
    <van-button block round plain type="info" @click="toAddressEdit">添加</van-button>
    </briup-fulllayout>
</template>

<script>
import {get,post} from '../../http/axios'
import {mapState} from 'vuex'
export default {
    data(){
        return{
            addresses:[],
        };
    },
    methods:{
        loadAddress(){
            let id =this.info.id;
            let url ="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.addresses =response.data;
            })
        },
        toAddressEdit(){
            this.$router.push("/manager/address_edit");
        },
    },
    created(){
        this.loadAddress();
    },
    computed:{ //计算属性
        //将状态机中的User对象中的info对象中获取到
        ...mapState("user",["info"]),
    }
}
</script>
<style scoped>
</style>