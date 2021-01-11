<template>
  <div class="hello">
  <home_top></home_top>
  <home_input></home_input>
  <home_body></home_body>
  <home_foot></home_foot>

  </div>
</template>
<script>
import home_top from "./home_top.vue"
import home_body from "./home_body.vue"
import home_foot from "./home_foot.vue"
import home_input from "./home_input.vue"

export default{
    components:{home_top, home_body,home_foot,home_input},
    watch:{
     $router:function(){
        this.$router.go(0)
      }


    }
  }

</script>

<style>

</style>
