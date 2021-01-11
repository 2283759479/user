<template>
  <div >
    <home_top></home_top>
    <div class="container" id="div1">
      <table class="table table-info table-hover" v-if="mycart.length!=0">
        <tr>
          <tr style="font-size: 20px;">
            <td>
              <input class="all" @click="checkAll()" type="checkbox" style="width: 30px;" />全选
            </td>
          <!-- <td @click="qx()"  class="quanx">{{quanx}}</td>
          <td @click="qbx()" style="display: none;color: red;" class="quanbx">{{quanx}}</td> -->
          <td>&emsp;&emsp;&emsp;图片</td>
          <td>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;名称</td>
          <td>尺码</td>
          <td>单价</td>
          <td>&emsp;&nbsp;数量</td>
          <td>总价</td>
          <td>移除商品</td>
        </tr>
        <tr v-for="v in mycart" class="tr1">
          <td class="td2">
            <input @click="check(2,v.gdid)" type="checkbox" v-if="v.ck==1" class="ck" />
            <input @click="check(1,v.gdid)" type="checkbox" v-if="v.ck==2" checked="checked" class="ck" />
          </td>
          <td class="td1">
            <img @click="jumpGoodsinfo(v)" :src="'../../static/tp/'+v.gimgurl" style="width: 112px;he height:170px;"/>
          </td>
          <td class="gdname">
            <span>{{v.gdname}}</span>
          </td>
          <td  class="td2">
            <span>{{v.gstext}}</span>
          </td>
          <td class="td2">

              <span>{{v.price}}</span>

          </td>
          <td class="td2">
             <button @click="jia(v.gdcount,1,v.gdid)" class="btn btn-success">+</button>
            <span>{{v.gdcount}}</span>
            <button @click="jia(v.gdcount,2,v.gdid)" class="btn btn-success">-</button>
          </td>
          <td class="td2">
            <span>￥{{v.gdcount*v.price}}</span>
          </td>
          <td>
            <button class="btn btn-danger" @click="delCart(v.gdid)">删除</button>
          </td>
        </tr>
      </table>

      <div v-else style="text-align: center;">
        <span style="font-size: 30px; color: red;">啥也不是</span><br />
        <img src="../../static/images/9.jpg" />
      </div>

      <div>
          <span style="float: right;margin-left: 30px;">总价：{{sum}}</span>
          <span style="float: right;" @click="addOrderinfo()">提交订单</span>
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
            mycart:[],
           /* quanx:"未全选", */
            sum:0,
         }
    },
    mounted() {
      this.useronline()
    },
    methods:{
     /* qx:function(){
          $(".quanbx").html("已全选");
          $(".quanbx").css("display","block");
           $(".quanx").css("display","none");
      },
      qbx:function(){
          $(".quanx").html("未全选");
          $(".quanx").css("display","block");
          $(".quanbx").css("display","none");
      }, */
      addOrderinfo:function(){
        var orders = [];
        for(var v in this.mycart){
          if(this.mycart[v].ck==2){
            orders.push(this.mycart[v]);
          }
        }
      
        this.$router.push({path:"/orderinfo",query:{orders:JSON.stringify(orders),price:this.sum}})
      },
      check:function(c,gdid){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/check",
          data:{ck:c,gdid:gdid},
          success: function(r) {
            o.queryCart();
            o.total();
          },
          xhrFields: {
            withCredentials: true //传递cookie,保持session的唯一性
          },
          crossDomain: true,
        })
      },
      checkAll:function(){
       var o = this;
       var ck = 2;
       if($(".all").prop("checked")){
          $(".ck").prop("checked",true);
       }else{
           $(".ck").prop("checked",false);
           ck = 1;
       }
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/check",
          data:{ck:ck},
          success: function(r) {
            o.queryCart();
            o.total();
          },
          xhrFields: {
            withCredentials: true //传递cookie,保持session的唯一性
          },
          crossDomain: true,
        })
      },

      jia:function(c,n,gdid){
        if(n==1){
          c++;
        }
        if(n==2){
          if(c>1){
            c--;
          }
        }
        var o = this;
          $.ajax({
            url: "http://127.0.0.1:8081/java2020/ajax/jia",
            data:{gdcount:c,gdid:gdid},
            success: function(r) {
              o.queryCart();
              o.total();
            },
            xhrFields: {
              withCredentials: true //传递cookie,保持session的唯一性
            },
            crossDomain: true,
          })
      },

      delCart:function(gdid){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/delCart",
          data:{gdid:gdid},
          success: function(r) {
            o.queryCart();
            o.total();
          },
          xhrFields: {
            withCredentials: true //传递cookie,保持session的唯一性
          },
          crossDomain: true,
        })
      },
      total:function(){
        var sum=0;
        for(var a in this.mycart){
          if(this.mycart[a].ck==2){
            sum+=this.mycart[a].price*this.mycart[a].gdcount;
          }
        }
        this.sum=sum;
      },
      //跳转
      jumpGoodsinfo:function(goodsinfo){
        this.$router.push({path:"/goodsinfo",query:goodsinfo})
      },
      queryCart:function(){
        var o = this;
          $.ajax({
            url: "http://127.0.0.1:8081/java2020/ajax/queryCart",
            success: function(r) {
              o.mycart=r;
              o.total();
            },
            xhrFields: {
              withCredentials: true //传递cookie,保持session的唯一性
            },
            crossDomain: true,
          })
      },
      //检测用户是否在线
      useronline:function(){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/useronline",
          success: function(r) {
            if(r==""){
              o.$router.push("/login");
            }else{
              o.queryCart();

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
  .td1 img{
   border: 5px solid skyblue;
   border-radius: 50%;
  }
  .tr1 td{
    line-height: 170px;

  }
  .gdname{
    text-align: center;
  }
  .td1 img:hover{
    transition: all 10s;
   /* transform: translateY(-50px); */
    transform: rotate(3600000deg);
    box-shadow: 0 0 10px black;
  }
</style>
