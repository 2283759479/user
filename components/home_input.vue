<template>
  <div>
      <div class="search">
        <input v-model="searchtext" @blur="hide()" @focus="show()"  @keyup="sousuo()"  /> <button @click="jumpSearch()">搜索</button>
        <ul class="sousuo">
         <li @click="check(v.gdname)" v-for="v in searchresult">
           {{v.gdname}}
         </li>
        </ul>
      </div>

  </div>
</template>

<script>
  export default{
    data:function(){
      return{
        searchtext: "", //搜索文本
        searchresult: [], //搜索结果

      }
    },

    mounted() {

      //搜提示对齐输入框
      var left = $(".search input").offset().left+"px";
       $(".sousuo").css("left",left);
      
      $(window).resize(function(){
        var left1 = $(".search input").offset().left+"px";
        $(".sousuo").css("left",left1);
       
      })
    },
    methods:{
      //搜索提示
      sousuo:function(){
        var o = this;
        $.ajax({
          url: "http://127.0.0.1:8081/java2020/ajax/sousuo",
          data: {gdname:o.searchtext},
          success: function(r) {
             o.searchresult = r;
          },
        })
      },
      //跳转搜索页面
      jumpSearch:function(){
        this.$router.push({path:"/searchGoods",query:{searchtext:this.searchtext}})
      },
      //显示提示
      show:function(){
        $(".sousuo").css("display","block");
      },
      //隐藏提示
      hide:function(){
        setTimeout(function(){
          $(".sousuo").css("display","none");
        },300)
      },
      //选择提示文本
      check:function(gdname){
        this.searchtext = gdname;
      },
     }
  }
</script>

<style>
  .sousuo{
    width: 500px;
    margin: 0 auto;
    list-style-type: none;
    z-index: 10;
    background-color: white;
    border: none;

    display: block;

  }
  input:focus ~.sousuo{
    display: block;
  }
  .sousuo li:hover{
    background-color: gainsboro;
  }
  .sousuo li{
      font-size: 18px;
      text-indent: 18px;
      padding: 5px 0px;
  }
  .search{
    margin-top: 50px;
    text-align: center;
  }
  .search input{
    width: 500px;
    height: 40px;
    outline: none;
    padding: 0;
    text-indent: 18px;
  }
  .search input,.search button{
    border: 1px solid gainsboro;
    font-size: 18px;
  }
  .search button{
    padding: 0;
    width: 60px;
    height: 42px;
    position: relative;
    right: 5px;
    color: white;
    background-color: skyblue;
  }

</style>
