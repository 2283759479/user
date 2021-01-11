<template>
  <div>
    <home_top></home_top>
    <div>
      <table class="table table-hover table-info table-striped">
        <tr>
          <td>订单编号</td>
          <td>下单日期</td>
          <td>订单状态</td>
          <td>地址</td>
          <td>收件人</td>
          <td>手机号</td>
          <td>操作</td>
        </tr>
        <tr v-for="v in md">
          <td>{{v.ofid}}</td>
          <td>{{v.ofdate}}</td>
          <td>
            <span v-if="v.ofstate==0">未付款</span>
            <span v-if="v.ofstate==1">已付款</span>
          </td>
          <td>{{v.address}}</td>
          <td>{{v.recipient}}</td>
          <td>{{v.contactnumber}}</td>
          <td>
            <span class="ddmx" @click="orderDetails(v.ofid)" data-toggle="modal" data-target=".bs-example-modal-lg">订单明细</span>
          </td>
        </tr>
      </table>
    </div>
    


<!-- Large modal -->


<div class="modal fade bs-example-modal-lg" tabindex="-1" role="dialog" aria-labelledby="myLargeModalLabel">
  <div class="modal-dialog modal-lg" role="document">
    <div class="modal-content">
     <table class="table table-hover table-info">
       <tr>
         <td>图片</td>
         <td>名称</td>
         <td>尺码</td>
         <td>价格</td>
         <td>数量</td>
       </tr>
       <tr class="trr" v-for="v in od">
         <td>
           <img @click="jumpGoodsinfo(v)" :src="'../../static/tp/'+v.gimgurl" style="width: 112px;height: 170px;"/>
         </td>
         <td>{{v.gdname}}</td>
     
         <td>{{v.gstext}}</td>
     
         <td>{{v.price}}</td>
         <td>{{v.gdcount}}</td>
       </tr>
     </table>
    </div>
  </div>
</div>



  </div>
</template>

<script>
  import home_top from "./home_top.vue"
  export default{
    components:{
      home_top
    },
    data:function(){
      return{
            md:[],
            od:[],
      }
    },
    mounted() {
      this.useronline()
    },
    methods:{
      jumpGoodsinfo:function(goodsinfo){
        this.$router.push({path:"/goodsinfo",query:goodsinfo})
      },
      orderDetails:function(ofid){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/orderDetails",
          data:{ofid:ofid},
          success: function(r) {
             o.od = r;
          },
          xhrFields: {
            withCredentials: true //传递cookie,保持session的唯一性
          },
          crossDomain: true,
        })
      },
      queryOrderinfo:function(){
       var o = this;
       $.ajax({
         url: "http://127.0.0.1:8081/java2020/ajax/queryOrderinfo",
         success: function(r) {
            o.md = r;
         },
         xhrFields: {
           withCredentials: true //传递cookie,保持session的唯一性
         },
         crossDomain: true,
       })
      },
      useronline:function(){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/useronline",
          success: function(r) {
            if(r==""){
              o.$router.push("/login");
            }else{
              o.queryOrderinfo();
            }
          },
          xhrFields: {
            withCredentials: true //传递cookie,保持session的唯一性
          },
          crossDomain: true,
        })
      },
    }
  }
</script>

<style scoped="">
  .ddmx{
    background-color: pink;
    border-radius: 5px;
    color: lightgoldenrodyellow;
  }
  .ddmx:hover{
    color: #000000;
  }
</style>
