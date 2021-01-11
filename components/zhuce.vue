<template>
  <div  class="zhu">
    <div class="zhuce">
      <h1>注册页面</h1>
      <div >
        <div class="txt"><input v-model="logname" type="text" placeholder="请输入您的用户名"/></div>
      </div>
      <div >
        <div class="txt"><input  v-model="password" type="password" placeholder="请输入您的密码" class="password"/></div>
      </div>
      <div >
        <input @click="zhuce()" type="submit" value="立即注册" />
      </div>
      <div >
        <a href="/login">登录</a>
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
           /* panduan:function(){
              if($(".password").value(""))
            } */
            zhuce:function(){
              var o=this;
              $.ajax({
                url:"http://127.0.0.1:8081/java2020/ajax/zhuce",
                data: {logname:o.logname,password:o.password},
                xhrFields: {
                  withCredentials: true //传递cookie,保持session的唯一性
                },
                crossDomain: true,
                success:function(r){
                  if(r>0){
                    alert("注册成功");
                    o.$router.push("/login");
                  }
                }
              })
            }
      }
    }
</script>

<style >
 .zhu{
    margin-top: 150px;
    text-align: center;

  }
  .zhuce{
    margin: 0 auto;
    border: 10px solid #47A3DA;
    width: 400px;
    background-color: #87CEEB;
  }
  .zhu input{
    margin-top: 20px;
  }
  .zhuce button{
    width:100px;
    margin-left: 90px;
    margin-right: 90px;
  }
</style>
