<template>
  <div class="pos">
      <el-container>
        <div class="order">
  <el-aside width="550px">  <el-tabs>
              <el-tab-pane label="点餐" >
                <el-table :data="tableData"  border show-summary style="width: 100%">
                           <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                                <el-table-column prop="count" label="数量" width="100"></el-table-column>
                                <el-table-column prop="price" center='true' label="金额" width="90"></el-table-column>
                                <el-table-column label="操作" width='120' >
                                    <template slot-scope='scope'>
                                        <el-button type='text' size='mini' @click="delSingleGoods(scope.row)">删除</el-button>
                                        <el-button type='text' size='mini' @click="addOrderList(scope.row)">增加</el-button>
                                    </template>
                                </el-table-column>
                            </el-table>
                            
                <div class='button'>
<el-button type="success" size='large' >挂单</el-button>
<el-button type="danger" @click="delAllGoods()">删除</el-button>
<el-button type="primary" @click="checkout()">结账</el-button>
</div>
              </el-tab-pane>
               <el-tab-pane label="挂单">挂单</el-tab-pane>
                <el-tab-pane label="外卖">外卖</el-tab-pane>
            </el-tabs></el-aside>
        </div>
          <el-main class='menu'>
<div class="often-goods">
    <div class="title">常用商品</div>
    <div class="often-goods-list"> 
        <ul>
            <li v-for='goods in oftenGoods' @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
            </li>
 
        </ul>
    </div>
</div>
  <div class="goods-type">
 
    <el-tabs>
        <el-tab-pane label="汉堡">
<ul class='cookList'>
    <li v-for='goods in type0Goods'  @click="addOrderList(goods)">
        <span class="foodImg"><img :src='goods.goodsImg' width='100%'></span>
        <span class="foodName">{{goods.goodsName}}</span>
        <span class="foodPrice">￥{{goods.price}}元</span>
    </li>
</ul>
        </el-tab-pane>
            <el-tab-pane label="小食">
            <ul class='cookList'>
    <li v-for='goods in type1Goods'  @click="addOrderList(goods)">
        <span class="foodImg"><img :src='goods.goodsImg' width='100%'></span>
        <span class="foodName">{{goods.goodsName}}</span>
        <span class="foodPrice">￥{{goods.price}}元</span>
    </li>
</ul>
        </el-tab-pane>
        <el-tab-pane label="饮料">
            <ul class='cookList'>
    <li v-for='goods in type2Goods' @click="addOrderList(goods)">
        <span class="foodImg"><img :src='goods.goodsImg' width='100%'></span>
        <span class="foodName">{{goods.goodsName}}</span>
        <span class="foodPrice">￥{{goods.price}}元</span>
    </li>
</ul>
        </el-tab-pane>
        <el-tab-pane label="套餐">
            <ul class='cookList'>
    <li v-for='goods in type3Goods' @click="addOrderList(goods)">
        <span class="foodImg"><img :src='goods.goodsImg' width='100%'></span>
        <span class="foodName">{{goods.goodsName}}</span>
        <span class="foodPrice">￥{{goods.price}}元</span>
    </li>
</ul>
        </el-tab-pane>
 
    </el-tabs>
</div>
          </el-main>
  </el-container>
  </div>
</template>
 
<script>
import axios from 'axios'
export default {
  name: 'Pos',
  data(){
     return {tableData: [],
        oftenGoods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],

        }
  }
  , created(){
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response=>{
         console.log(response);
         this.oftenGoods=response.data;
      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      })
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response=>{
         console.log(response);
         //this.oftenGoods=response.data;
         this.type0Goods=response.data[0];
         this.type1Goods=response.data[1];
         this.type2Goods=response.data[2];
         this.type3Goods=response.data[3];
 
      })
      .catch(error=>{
          console.log(error);
          alert('网络错误，不能访问');
      })
  },

methods:{
      //添加订单列表的方法
      addOrderList(goods){
            this.totalCount=0; //汇总数量清0
            this.totalMoney=0;
            let isHave=false;
            //判断是否这个商品已经存在于订单列表
            for (let i=0; i<this.tableData.length;i++){
                console.log(this.tableData[i].goodsId);
                if(this.tableData[i].goodsId==goods.goodsId){
                    isHave=true; //存在
                }
            }
            //根据isHave的值判断订单列表中是否已经有此商品
            if(isHave){
                //存在就进行数量添加
                 let arr = this.tableData.filter(o =>o.goodsId == goods.goodsId);
                 arr[0].count++;
                 //console.log(arr);
            }else{
                //不存在就推入数组
                let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
                 this.tableData.push(newGoods);
 
            }
 
            //进行数量和价格的汇总计算
            this.tableData.forEach((element) => {
                this.totalCount+=element.count;
                this.totalMoney=this.totalMoney+(element.price*element.count);   
            });
           
      },
      delSingleGoods(goods){
        console.log(goods);
        this.tableData=this.tableData.filter(o => o.goodsId !=goods.goodsId);
 
      },
   delAllGoods() {
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;
        },

checkout() {
    if (this.totalCount!=0) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
            message: '结账成功，感谢你又为店里出了一份力!',
            type: 'success'
        });
 
    }else{
        this.$message.error('不能空结。老板了解你急切的心情！');
    }
 
}
  }
}
</script>
<style scoped>
.menu{
  margin:0px;
  padding:0px;
  border-left:2px solid #ccc;
  height:960px;
}
.often-goods{
  margin-bottom:20px;
}
.often-goods:after{
content: '';
display: block;
clear: both;
}
.order{
  background-color: #fff;
  margin-left:5px;
  height:100%;
  margin:0px;
  padding: 0px;
}
.goods-type{
margin-left:4px;
}

 .title{
   margin:0px;
   padding:0px;
       height: 20px;
       border-bottom:1px solid #ccc;
       background-color: #F9FAFC;
       padding:10px;
   }
   .often-goods-list ul li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:10px;
      background-color:#fff;
   }
  .o-price{
      color:#58B7FF; 
   }
.button{
  margin-top:25px
}
.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
   }
   .cookList li span{
       
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
</style>