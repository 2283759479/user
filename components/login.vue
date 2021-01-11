<template>
 <div class="deng">
   <div class="denglu" >
     <h1>欢迎回来</h1>
     <div >
       <div class="txt"><input v-model="logname" type="text" placeholder="请输入您的用户名"/></div>
     </div>
     <div >
       <div class="txt"><input @keydown.13="login()" v-model=" password"type="password" placeholder="请输入您的密码"/></div>
     </div>
     <div >
       <input @click="login()"type="submit" value="安全登录" />
     </div>
     <div >
       <a href="/zhuce">立即注册</a>
       <a href="/home">首页</a>
     </div>
     </div>

   </div>
 </div>
</template>

<script>

 export default{
     data:function(){
       return{
         logname:"",
         password:"",
       }
     },
     methods:{
       login:function(){
         var o = this;
         $.ajax({ //
           url: "http://127.0.0.1:8081/java2020/ajax/login",
           data:{logname:o.logname,password:o.password},
           success: function(r) {
             if(r){
               $.ajax({
                 url: "http://127.0.0.1:8081/java2020/ajax/useronline",
                 success: function(r) {
                   o.$router.push("/home")
                 },
                 xhrFields: {
                   withCredentials: true //传递cookie,保持session的唯一性
                 },
                 crossDomain: true,
               })

             }else{
               alert("用户名或密码错误！")
             }
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
</script>

<style>
  .deng{
    margin-top: 150px;
    text-align: center;

  }
  .denglu{
    margin: 0 auto;
    border: 1px solid #47A3DA;
    width: 400px;
    background-color: #87CEEB
  }
  .deng input{
    margin-top: 20px;
  }
  .denglu button{
    width:100px;
    margin-left: 90px;
    margin-right: 90px;
  }

</style>
