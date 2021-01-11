<template>
  <div >

    <home_top></home_top>

 <!-- Modal -->
    <div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h4 class="modal-title" id="myModalLabel">修改信息</h4>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>

          </div>

          <div class="modal-body">

            <linkage></linkage>
            <hr />

            <div>详细地址：<br />
              <textarea v-model="ar.dAddress" style="width: 100%;" class="form-control"></textarea>
            </div>

            <div>收件人：<br />
              <input v-model="ar.linkpeople" class="form-control" />
            </div>

            <div>联系电话：<br />
              <input v-model="ar.phone" type="number" class="form-control"/>
            </div>
          </div>

         <div class="modal-footer">
            <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
            <button type="button" class="btn btn-primary"@click="updateAddress()">保存</button>
          </div>
        </div>
      </div>
    </div>


   <div style="text-align: center;">

     <label v-for="v in address" class="address" style="margin-right: 10px;margin-bottom: 10px;">
        <input hidden="hidden" type="radio" name="address" />
       <div class="address" @click="orderAddress=v">

       <span >{{v.address}}</span>

       <span>{{v.linkpeople}}(收)</span>
        <br />
        <hr />
       <span>{{v.dAddress}}</span>
       <span>{{v.phone}}</span>
       <br />
       <br />
       <span @click="fuzhiAddress(v)" class="xiugai" style="margin-right: 50px;background-color: #FFC0CB;border-radius: 5px;color: white;"data-toggle="modal" data-target="#myModal">修改</span>
       <span @click="delAddress(v.aid)" class="shanchu" style="background-color: red; border-radius: 5px;color: white;" >删除</span>
     </div>
     </label>

   </div>

    <br />
    <div style="text-align: center;">
      <span class="tianjia" @click="jumpAddaddress()" style="font-size: 30px;color: pink;">添加收货地址</span>
    </div>

    <div class="xinxi container">
      <table class="table table-hover table-info">
        <tr>
          <td>图片</td>
          <td>名称</td>
          <td>尺码</td>
          <td>价格</td>
          <td>数量</td>
        </tr>
        <tr class="trr" v-for="v in orderinfo">
          <td>
            <img @click="jumpGoodsinfo(v)" :src="'../../static/tp/'+v.gimgurl" style="width: 112px;height: 170px;"/>
          </td>
          <td>{{v.gdname}}</td>

          <td>{{v.gstext}}</td>

          <td>{{v.price}}</td>
          <td>{{v.gdcount}}</td>
        </tr>
      </table>
      <button @click="submitOrder()" class="btn btn-info" style="float: right;margin-left: 20px;" >付款</button>
      <span style="float: right;font-size: 20px;color: pink;">总价:{{price}}</span>
    </div>




  </div>
</template>

<script>
  import home_top from "./home_top.vue"
  import linkage from "./linkage.vue"
  export default{
    components:{
      home_top,linkage
    },
    data:function(){
     return{
        orderinfo:JSON.parse(this.$route.query.orders),
        price:this.$route.query.price,
        address:[],
        ar:{dAddress:"", phone:"",linkpeople:""},
        price:this.$route.query.price,
        orderAddress:null,
     }

    },
    mounted() {
      this.useronline()
    },
    methods:{
      fuzhiAddress:function(v){
        this.ar = v;
      },
      jumpGoodsinfo:function(goodsinfo){
        this.$router.push({path:"/goodsinfo",query:goodsinfo})
      },
      jumpAddaddress:function(){
        this.$router.push({path:"/addAddress",query:{orders:JSON.stringify(this.orderinfo)}})

      },
      submitOrder:function(){
         var o = this;
        if(this.orderAddress!=null){
         
          var ar = $("#province").val()+$("#city").val()+$("#area").val();
          $.ajax({
            url: "http://127.0.0.1:8081/java2020/submitOrder",
            data:{address:ar+o.orderAddress.address,recipient:o.orderAddress.linkpeople,
             contactnumber:o.orderAddress.phone,orders:JSON.stringify(o.orderinfo)},
            success: function(r) {
              window.open("http://127.0.0.1:8081/java2020/ali/pay?orderid="+r.ofid+"&price="+r.price)

            },
            xhrFields: {
              withCredentials: true //传递cookie,保持session的唯一性
            },
            crossDomain: true,
          })
        }else{
          alert("请选择收货地址")
        }
      },
      delAddress:function(aid){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/delAddress",
          data:{aid:aid},
          success: function(r) {
             o.queryAddress();

          },
          xhrFields: {
            withCredentials: true //传递cookie,保持session的唯一性
          },
          crossDomain: true,
        })
      },
      updateAddress:function(){
        var ar = $("#province").val()+$("#city").val()+$("#area").val();
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/updateAddress",
          data:{address:ar,dAddress:o.ar.dAddress,linkpeople:o.ar.linkpeople,phone:o.ar.phone,aid:o.ar.aid},
          success: function(r) {
              o.queryAddress();

          },
          xhrFields: {
            withCredentials: true //传递cookie,保持session的唯一性
          },
          crossDomain: true,
        })
      },
      queryAddress:function(){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/queryAddress",
          success: function(r) {
             o.address=r;

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

              o.queryAddress();
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
  .address{
    background-color: skyblue;
    width: 250px;
    border: 2px solid pink;
   height: 180px;
   text-align: center;
   display: inline-block;

  }
  .xinxi{
    margin-top: 30px;
  }
  .xiugai:hover{
    cursor: pointer;
  }
  .shanchu:hover{
     cursor: pointer;
  }
  .tianjia:hover{
     cursor: pointer;
  }
  [name="address"]:checked+.address{
    background-color: pink;
  }
  .address{
    transition: all 1s;
  }
  .trr{
    line-height: 170px;
  }
</style>
