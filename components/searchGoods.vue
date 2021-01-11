<template>
  <div>
    <home_top></home_top>
    <home_input></home_input>
    <div class="queryprice">
      <label style="color: cornflowerblue;">
        最低价:<input v-model="minprice" type="number" style="width: 50px;"/>
      </label>
      <label style="color: cornflowerblue;">
       <input v-model="maxprice" type="number"style="width: 50px;"/> 最高价
      </label>
      <button style="padding: 5px;background-color:skyblue;
      border:none;border-radius: 5px;" @click="queryByprice(1,minprice,maxprice)">搜索</button>
      <span @click="searchGoods(1,'韩版')" style="margin-left: 150px;">韩版</span>
      <span @click="searchGoods(1,'休闲')">休闲</span>
      <span @click="searchGoods(1,'短款')">短款</span>
      <span @click="searchGoods(1,'百搭')">百搭</span>
      <span @click="searchGoods(1,'牛仔')">牛仔</span>
      <span @click="searchGoods(1,'针织')">针织</span>
      <span @click="searchGoods(1,'雪纺')">雪纺</span>
      <span @click="searchGoods(1,'刺绣')">刺绣</span>
      <span @click="searchGoods(1,'文艺')">文艺</span>
      <span @click="searchGoods(1,'棉麻')">棉麻</span>
    </div>

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
    <ul class="pagination" v-if="flag">
      <li @click="searchGoods(1,searchtext)" class="page-item"><a class="page-link">首页</a></li>
      <li @click="searchGoods(pn-1,searchtext)" class="page-item" v-if="pn!=1"><a class="page-link">上一页</a></li>
      <li @click="searchGoods(v,searchtext)" class="page-item active" v-if="v==pn" v-for="v in pagenum"><a class="page-link">{{v}}</a></li>
      <li @click="searchGoods(v,searchtext)" class="page-item" v-else><a class="page-link">{{v}}</a></li>
      <li @click="searchGoods(pn+1,searchtext)" class="page-item" v-if="pn!=zy"><a class="page-link">下一页</a></li>
      <li @click="searchGoods(zy,searchtext)" class="page-item"><a class="page-link">尾页</a></li>
    </ul>
    <ul class="pagination" v-else>
    	<li style="font-size: 20px;" @click="queryByprice(1,minprice,maxprice)" class="page-item"><a class="page-link" href="#">首页</a></li>
    	<li style="font-size: 20px;" @click="queryByprice(pn-1,minprice,maxprice)" v-if="pn!=1" class="page-item"><a class="page-link" href="#">上一页</a></li>

    	<li style="font-size: 20px;" @click="queryByprice(v,minprice,maxprice)" v-if="v==pn" v-for="v in pagenum" class="page-item active"><a class="page-link"
    		 href="#">{{v}}</a></li>
    	<li style="font-size: 20px;" @click="queryByprice(v,minprice,maxprice)" v-else class="page-item"><a class="page-link" href="#">{{v}}</a></li>

    	<li style="font-size: 20px;" @click="queryByprice(pn+1,minprice,maxprice)" v-if="pn!=zy" class="page-item"><a class="page-link" href="#">下一页</a></li>
    	<li style="font-size: 20px;" @click="queryByprice(zy,minprice,maxprice)" class="page-item"><a class="page-link" href="#">尾页</a></li>
    </ul>
  </div>
</template>

<script>
  import home_top from "./home_top.vue"
  import home_input from "./home_input.vue"
  export default{
    components:{home_top,home_input},
    data:function(){
      return{
        searchtext:this.$route.query.searchtext,
        goodsinfo:[],
        pagenum:[],
        flag:true,
        pn:0,
        zy:0,
        minprice:0,//最低价
        maxprice:0,//最高价
      }
    },
    watch:{
     '$route':function(){
       this.$router.go(0)
     }
    },
    mounted() {
      this.searchGoods(1,this.searchtext)
    },
    methods:{
      jumpGoodsinfo:function(goodsinfo){
        this.$router.push({path:"/goodsinfo",query:goodsinfo})
      },
    queryByprice:function(pn,minprice,maxprice){
      var obj = this;
      obj.flag =false;
      obj.minprice = minprice;
      obj.maxprice = maxprice;
      $.ajax({
        url: "http://127.0.0.1:8081/java2020/ajax/queryByprice",
        data:{minprice:obj.minprice,maxprice:obj.maxprice,pn:pn},
        success: function(r) {
         obj.goodsinfo = r.goods;
         obj.zy = r.zy;
         obj.pn = r.pn;
         //当前页码大于15页时 将第一段最大页码赋值为15
         var newzy = r.zy;
         if(r.zy>=15){
         	newzy = 15;
         }

         var k = r.pn < 7 ? 1 : r.pn - 7; //开始页码
         var j = r.pn < 7 ? newzy : r.pn + 7; //结束页码
         j = r.pn + 7 > r.zy ? r.zy : j; //当结束页码+7大于最大页码时,终止页码的相加
         obj.pagenum = []; //清除上一次的页码
         //将页码存储到数组中
         for (var i = k; i <= j; i++) {
         	obj.pagenum.push(i);
         }
        },
      })
    },
      searchGoods:function(pn,gdname){
          var o=this;
          o.searchtext=gdname;
          $.ajax({
            url:"http://127.0.0.1:8081/java2020/ajax/searchGoods",
            data:{gdname:o.searchtext,pn:pn},
            success:function(r){
              o.goodsinfo=r.goods;
              o.pn=r.pn;
              o.zy=r.zy;
              var newzy=r.zy;
              if(r.zy>=15){
                newzy=15;
              }
            var k = r.pn < 7 ? 1 : r.pn - 7; //开始页码
            var j = r.pn < 7 ? newzy : r.pn + 7; //结束页码
            j = r.pn + 7 > r.zy ? r.zy : j; //当结束页码+7大于最大页码时,终止页码的相加
            o.pagenum = []; //清除上一次的页码
            //将页码存储到数组中
            for (var i = k; i <= j; i++) {
            	o.pagenum.push(i);
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
  .queryprice span{
    font-size: 20px;
    background-color: pink;
    border-radius: 5px;
    border: 1px solid skyblue;
    margin-right: 30px;
  }
  .queryprice span:hover{
    background-color: skyblue;
  }
</style>
