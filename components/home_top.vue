<template>
  <div>
    <ul class="ul_nav">
      <li>
          <router-link to="/home">首页</router-link>
      </li>
      <li>
          <router-link to="/cart">购物车</router-link>
      </li>
      <li>
          <router-link to="/mycollection">收藏夹</router-link>
      </li>
      <li>
          <router-link to="/myorder">我的订单</router-link>
      </li>
      <li>
          <router-link  to="/zhuce">注册</router-link>
      </li>
      <li>
          <router-link v-if="online==''" to="/login">登录</router-link>
          <span class="logname" style="color: #00BFFF;" v-else>{{online.logname}}</span>
          <ul class="usercase"><!-- userinfo.name -->
            <li>
              <router-link to="#">修改头像</router-link>
            </li>
            <li>
              <a href="#" @click="personal()">个人信息</a>
            </li>
            <li>
              <a href="http://127.0.0.1:8081/java2020/logout">退出登录</a>
            </li>
          </ul>
      </li>
    </ul>
    <!-- 个人信息窗口-->
    <div class="personal" style="text-align: left;">
      <span @click="closeWindow()" class="closeX">&times;</span><br />

      昵&emsp;称：<input readonly type="text" v-model="userinfo.name" /><br /><br />

      性&emsp;别：<label>男 <input readonly v-model="userinfo.sex" value="0" type="radio"/></label>
      <label>女 <input readonly v-model="userinfo.sex" value="1" type="radio"/></label><br /><br />

      生&emsp;日：<input readonly type="date" v-model="userinfo.birthday" /><br /><br />

      手机号：<input readonly type="text" v-model="userinfo.ufnum" /><br /><br />


      <button @click="update()" class="btn btn-info update">修改</button>
    </div>
  </div>
</template>

<script>
  export default{
    data:function(){
      return{
        online:"", // 用户在线状态
        userinfo:"",// 用户的个人信息
      }
    },
    mounted() {

      this.useronline()
    },
   methods:{
  update:function(){
        var o = this;
        if($(".update").text()=="修改"){
            $(".personal input").css("border-bottom","1px solid skyblue");
            $(".personal input").prop("readonly",false);
            $(".update").text("保存");
        }else{
            $.ajax({
              url: "http://127.0.0.1:8081/java2020/ajax/update",
              data: o.userinfo,
              success: function(r) {
                 o.personal();
                 $(".update").text("修改");
                 $(".personal input").css("border-bottom","none");
                 $(".personal input").prop("readonly",true);
              },
            })
        }
    },
  personal:function(){
      var userid = this.online.userid;
      var o = this;
      $.ajax({
        url: "http://127.0.0.1:8081/java2020/ajax/personal",
        data: {userid:userid},
        success: function(r) {
           o.userinfo = r;
           if(o.userinfo.birthday!=null){
             o.userinfo.birthday = o.userinfo.birthday.substr(0,10)
           }


           $(".personal").css("transform","scale(1)")
        },
      })

  },
      closeWindow:function(){
             $(".personal").css("transform","scale(0)")
        },

        useronline:function(){
          var o = this;
          $.ajax({
            url: "http://127.0.0.1:8081/java2020/ajax/useronline",
            success: function(r) {

               o.online = r;
            },
            xhrFields: {
              withCredentials: true //传递cookie,保持session的唯一性
            },
            crossDomain: true,
          })
        }
  }
}
</script>

<style>
  .usercase{
    list-style-type: none;
    background-color: white;
    border: 1px solid gainsboro;
    position: absolute;
    display: none;
    padding: 5px;
    z-index: 10;

  }
  .usercase>li a{
    display: block;
    color: #20BDFE;
    padding: 5px 0px;
  }
  .logname:hover + .usercase{
    display: block;
  }
  .usercase:hover{
    display: block;
  }
  .personal{
      width: 30%;
      background-color: skyblue;
      margin-left: 35%;
      position: absolute;
      z-index: 111;
      padding: 20px;
      transform: scale(0);
      transition: all 0.3s;

  }
  .personal input{
        background-color: transparent;
        border: none;
        outline: none;
   }

  .closeX{
    position: absolute;
    right: 20px;
    font-size: 30px;
    top: -10px;
    cursor: pointer;
    color: #1D1A44;
  }

 .ul_nav>li{
   display: inline-block;
   list-style-type: none;
   padding: 5px 10px;
   font-size: 10px;
 }
 .ul_nav>li a{
   text-decoration: none;

 }
 .ul_nav>li a:hover{
   text-decoration: underline;
   color: blueviolet;
 }
 .ul_nav{
   border-bottom: 1px solid skyblue;
   padding: 0;
   margin-top: 0;
 }
 .sousuo{
 border: 1px solid black;
  margin-left: 500px;
  position: absolute;
  list-style-type: none;
  z-index: 10;
  background-color: white;
  border: none;
  text-align: left;
  display: none;
 }
 input:focus ~.sousuo{
   display: block;

 }
 .sousuo li:hover{
   background-color: aqua;
 }
 .search{
   margin-top: 30px;
   text-align: center;
 }
 .search input{
   width: 500px;
   height: 40px;
   outline: none;
   padding: 0;
   text-indent: 18px;
   margin-bottom: 30px;
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
