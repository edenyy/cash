<template>
  <div class="left-nav">
      <ul>
          <li>
              <i class="iconfont icon-shouyintai"></i>
              <div>收银</div>
          </li>
             <li>
              <i class="iconfont icon-msnui-shop"></i>
              <div>店铺</div>
          </li>
             <li>
              <i class="iconfont icon-hanbao"></i>
              <div>商品</div>
          </li>
               <li>
              <i class="iconfont icon-huiyuan21"></i>
              <div>会员</div>
          </li>
               <li>
              <i class="iconfont icon-tongji"></i>
              <div>统计</div>
          </li>  
           
      </ul>
  </div>
</template>
<script>
    export default{
  name: 'Main',
  data () {
    return {
      
    }
  }
    }
</script>
<style>
     .left-nav{
       color:#fff;
       font-size:10px;
       height:100%;
       background-color: #1D8ce0;
       float:left;
       width:4%;
    }
    .iconfont{
       font-size:30px;
    }
    .left-nav ul{
        padding:0px;
        margin: 0px;
    }
    .left-nav li{
        list-style: none;
        border-bottom:1px solid #fff;
        padding:10px;
    }
</style>
