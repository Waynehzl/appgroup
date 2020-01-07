<template>
    <div>
        <briup-fulllayout title="订单确认">
        服务地址：
        <!-- <p> {{addressId}}</p> -->
        <van-dropdown-menu>
  <van-dropdown-item v-model="addressId" :options="options" />
</van-dropdown-menu>
订单详情:
<div style="padding:0 2em">
    <p>服务名称：{{$route.query.name}}</p>
    <p>服务简介：{{$route.query.description}}</p>
    <p>服务价格：{{$route.query.price}}</p>
    <p>服务数量：1</p>
    <p>服务小计：{{$route.query.price * 1 }}</p>
</div>
<div style="position:fixed;bottom:0;width:100%;">
<van-button block  @click="submitHandler" type="primary">提交订单</van-button>
</div>
        </briup-fulllayout>
    </div>
</template>

<script>
import {get,post_obj_array} from '../../../http/axios'
import { mapState } from 'vuex'
export default {
    data(){
        
        return{
        addressId:0,
        address:[],
        orderLines:[],
        
        }
    },
    created(){
        //在页面加载时查询地址信息
        this.loadAddress();
        //初始化订单项（将我们购买的产品放入到购物车中）
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
            //将address的数据计算为一个数组返回
            return this.address.map(item=>{
                return{
                    text:item.province+" "+item.city+" "+item.area+" "+item.address,
                    value:item.id
                }
            })
        }
    },
    methods:{
        submitHandler(){
            let url ="/order/save";
            let data= {
                customerId:this.info.id,
                addressId:this.addressId,
                orderLines:this.orderLines
            }
            console.log(data);
            post_obj_array(url,data).then(response =>{
                this.$router.push({
                    path:'order'
                })
            })
        },
        loadAddress(){
            let id =this.info.id;
            let url = "/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                //将查询到的地址信息绑定到address这个变量中
            this.address = response.data;
            //将查询到的地址列表中第一个地址id
            this.addressId=this.address[0].id;
            })
        }
     }
}
</script>