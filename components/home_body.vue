<template>
    <div style="background-color: transparent; height: 450px;overflow: hidden;">

      <div id="demo" class="carousel slide" data-ride="carousel">

        <!-- 指示符 -->
        <ul class="carousel-indicators">
          <li data-target="#demo" data-slide-to="0" class="active"></li>
          <li data-target="#demo" data-slide-to="1"></li>
          <li data-target="#demo" data-slide-to="2"></li>
        </ul>

        <!-- 轮播图片 -->
        <div class="carousel-inner">
          <div class="carousel-item active">
            <img src="../../static/lbt/1.jpg">
          </div>
          <div class="carousel-item">
            <img src="../../static/lbt/2.jpg">
          </div>
          <div class="carousel-item">
            <img src="../../static/lbt/3.jpg">
          </div>
        </div>

        <!-- 左右切换按钮 -->
        <a class="carousel-control-prev" href="#demo" data-slide="prev">
          <span class="carousel-control-prev-icon"></span>
        </a>
        <a class="carousel-control-next" href="#demo" data-slide="next">
          <span class="carousel-control-next-icon"></span>
        </a>

      </div>
    </div>
</template>

<script>
</script>

<style scoped="">

  #demo{
     width: 1222px;
     margin: 0 auto;

  }
  #demo img{
    width: 100%;
    height: 450px;
  }
  
</style>
