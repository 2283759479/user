<template>
  <div>
   <home_top></home_top>
   <home_input></home_input>
   <div class="goods">
      <div class="goodsinfo" v-if="(i+1)%5!=0" v-for="v,i in myCollection">
        <img @click="jumpGoodsinfo(v)" :src="'../../static/tp/'+v.gimgurl" />
        <span>{{v.gdname}}</span><br />
        <span>￥{{v.price}}</span>
      </div>
      <div class="goodsinfo not" v-else   >
        <img @click="jumpGoodsinfo(v)" :src="'../../static/tp/'+v.gimgurl" />
        <span>{{v.gdname}}</span><br />
        <span>￥{{v.price}}</span>
      </div>
   </div>
  </div>
</template>

<script>
 import home_top from "./home_top.vue"
 import home_input from "./home_input.vue"
 export default{
   components:{home_top,home_input},
   data:function(){
     return{
       myCollection:[],
     }
   },
   mounted() {
     this.useronline()
   },
  methods:{
    jumpGoodsinfo:function(goodsinfo){
      this.$router.push({path:"/goodsinfo",query:goodsinfo})
    },
     queryCollection:function(){
       var o = this;
       $.ajax({
         url: "http://127.0.0.1:8081/java2020/ajax/queryCollection",
         success: function(r) {
           o.myCollection = r;
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
             o.queryCollection()

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

<style scoped="" >
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
    border: 5px solid skyblue;
    background-color:skyblue;
    color: white;
  }
  .goodsinfo:hover{
    transition: all 100s linear;
    transform: rotate(3600000deg);
  }
   .goodsinfo img{
     width: 100%;
   }
  .goodsinfo:not(.not){
    margin-right: 1.2%;
  }
</style>
