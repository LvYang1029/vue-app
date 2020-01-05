<template>
    <briup-fulllayout title="常用地址">
    {{list}}
        <van-address-list
            v-model="chosenAddressId"
            :list="list"
            :disabled-list="disabledList"
            disabled-text="以下地址超出配送范围"
            default-tag-text="默认"
            @add="onAdd"
            @edit="onEdit"
        />
    </briup-fulllayout>
</template>
<script>
export default {
    data(){
        return{
            chosenAddressId: '1',
            list:[],
            disabledList: [
            {
                id: '3',
                name: '王五',
                tel: '1320000000',
                address: '浙江省杭州市滨江区江南大道 15 号'
            }
        ],
            address:{},
        };
    },
    methods:{
        loadAddress(){
            let id =26;
            let url ="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.address =response.data;
                for (let i in address) {
                    let o = {};
                    o[i] = address[i];
                    list.push(o);
                }

            })
        },
        onAdd() {
            Toast('新增地址');
        },

        onEdit(item, index) {
            Toast('编辑地址:' + index);
        }
    },
}
</script>
<style scoped>
</style>