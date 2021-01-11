<template>
  <div>
    <home_top></home_top>
    <div class="spxq">
      <div  class="img">
        <img :src="'../../static/tp/'+goodsinfo.gimgurl" />
      </div>

      <div class="gdname">
        <span style="font-size: 30px; margin-left: 20px;">
          {{goodsinfo.gdname}}
        </span>
      </div>
      <br />
      <div class="price">
        <span style="margin-left: 20px;font-size: 20px;">
          价格：￥{{goodsinfo.price}}
        </span>
      </div>
      <div>尺码：
      <label  v-for="v in goodsSize">
        <input v-if="v.gsid==7" checked="checked" hidden="hidden" type="radio" name="size" />
        <input v-else  hidden="hidden" type="radio" name="size" />
        <span @click="gsid=v.gsid" class="size">{{v.gstext}}</span>
      </label>

      </div>
      <br />
      <div>数量：
        <button class="btn btn-info" @click="gdcount++">+</button>
        <span>&nbsp;{{gdcount}}&nbsp;</span>
        <button class="btn btn-info"  @click="gdcount==1?1:gdcount--">-</button>
      </div>
      <br />
      <div>收藏：
        <span v-if="isC" @click="addCollection()" style="color: skyblue;">未收藏</span>
        <span v-else @click="delCollection()" style="color: red;">已收藏</span>
      </div>
      <br />

      <div>
        <button @click="addCart()" class="btn btn-danger" style="width: 380px;margin-left: 30px;">添加到购物车</button>
      </div>
    </div>
  </div>
</template>

<script>
  import home_top from "./home_top.vue"
  export default{
    components:{home_top},
    data:function(){
      return{
        goodsinfo:this.$route.query,
        goodsSize:[],
        gdcount:1,
        gsid:7,
        isC:true,
      }
    },
    mounted() {
      this.useronline()
    },
    //购物车
    methods:{
 //添加购物车
         addCart:function(){
             var o = this;
             $.ajax({
               url: "http://127.0.0.1:8081/java2020/ajax/isCart",
               data: {gdid:o.goodsinfo.gdid,gsid:o.gsid},
               success: function(r) {
                 if(r){
                   $.ajax({
                     url: "http://127.0.0.1:8081/java2020/ajax/addCart",
                     data: {gdid:o.goodsinfo.gdid,gdcount:o.gdcount,gsid:o.gsid,ck:1},
                     success: function(r) {
                       if(r>0){
                         alert("添加成功");
                       }
                     },
                     xhrFields: {
                       withCredentials: true //传递cookie,保持session的唯一性
                     },
                     crossDomain: true,
                   })
                 }else{
                   alert("该商品已经添加到购物车")
                 }
               },
               xhrFields: {
                 withCredentials: true //传递cookie,保持session的唯一性
               },
               crossDomain: true,
             })



         },
         //取消收藏
         delCollection:function(){
           var o = this;
           $.ajax({
             url: "http://127.0.0.1:8081/java2020/ajax/delCollection",
             data: {gdid:o.goodsinfo.gdid},
             success: function(r) {
               if(r>0){
                 alert("取消收藏");
                 o.isCollection();
               }
             },
             xhrFields: {
               withCredentials: true //传递cookie,保持session的唯一性
             },
             crossDomain: true,
           })
         },
         //添加收藏
         addCollection:function(){
           var o = this;
           $.ajax({
             url: "http://127.0.0.1:8081/java2020/ajax/addCollection",
             data: {gdid:o.goodsinfo.gdid},
             success: function(r) {
               if(r>0){
                 alert("收藏成功");
                 o.isCollection();
               }
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
                 o.queryGoodsSize();
                 o.isCollection();
               }
             },
             xhrFields: {
               withCredentials: true //传递cookie,保持session的唯一性
             },
             crossDomain: true,
           })
         },
         //检测商品是否被收藏
         isCollection:function(){
             var o = this;
             $.ajax({
               url: "http://127.0.0.1:8081/java2020/ajax/isCollection",
               data:{gdid:o.goodsinfo.gdid},
               success: function(r) {
                  o.isC = r;
               },
               xhrFields: {
                 withCredentials: true //传递cookie,保持session的唯一性
               },
               crossDomain: true,
             })
         },
         //查询尺码
         queryGoodsSize:function(){
           var o = this;
           $.ajax({
             url: "http://127.0.0.1:8081/java2020/ajax/queryGoodsSize",
             success: function(r) {
                for(var i in  r){
                  o.goodsSize = r;
                }
             },
           })
         },
    }
  }


</script>

<style scoped="">
  .spxq{
    margin-left: 30%;
    width: 700px;
    height: 400px;
    border: 10px solid skyblue;
    padding: 20px;
    position: absolute;
  }
  .img{
  float: left;
   }
 .size{
   display: inline-block;
   border: 1px solid gainsboro;
   padding: 5px 10px;
   margin-left: 11px;
   margin-top: 20px;
 }
 [type=radio]:checked+.size{
    border: 1px solid skyblue;
 }
</style>

 