<template>
  <div class="container goods">
   <div class="goodsinfo" v-if="(i+1)%5!=0" v-for="v,i in goodsinfo" >
     <img @click="jumpGoodsinfo(v)" :src="'../../static/tp/'+ v.gimgurl"/><br />
     <span>{{v.gdname}}</span><br />
     <span>￥{{v.price}}</span>
   </div>
   <div class="goodsinfo not" v-else   >
     <img @click="jumpGoodsinfo(v)" :src="'../../static/tp/'+v.gimgurl" /><br />
     <span>{{v.gdname}}</span><br />
     <span>￥{{v.price}}</span>
   </div>
  </div>
</template>

<script>
  var vm = null;
  $(function(){
      var i = 2;
      //判断滚动条到底部加载商品
      $(window).scroll(function(){
        if ($(document).scrollTop() >= $(document).height() - $(window).height()-20) {
            vm.queryGoodsinfo(parseInt(Math.random()*35)+1);
        }
      })
  })
  export default{
    name:"home_foot",
    data:function(){
      return{
        goodsinfo:[],
      }
    },
    mounted() {
      vm = this;
      this.queryGoodsinfo(parseInt(Math.random()*35)+1)
    },
    methods:{
      //跳转商品详情页面
      jumpGoodsinfo:function(goodsinfo){
        this.$router.push({path:"/goodsinfo",query:goodsinfo})
      },
      queryGoodsinfo:function(pn){
        var o=this;
        $.ajax({
          url:"http://127.0.0.1:8081/java2020/ajax/queryGoodsinfo",
          data:{pn:pn},
          success:function(r){
            for(var i in r){
              o.goodsinfo.push(r[i]);
            }
          }
        })
      }
    }

  }
</script>

<style scoped="">
  .goods{
    width:1222px;
    margin: 0 auto;
  }
  .goodsinfo{
    display: inline-block;
    width: 19%;
    padding: 5px;
    text-align: center;
    margin-bottom: 50px;
    color: black;

  }
  .goodsinfo:hover{
    background-color: red;
    transition: all 0.3s;
    transform: translateY(-5px);
    box-shadow: 0 0 10px black;
  }
   .goodsinfo img{
     width: 100%;
   }
  .goodsinfo:not(.not){
    margin-right: 1.2%;
  }
</style>
