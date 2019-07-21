<template>
  <div class="container">
      <!-- 面包屑 -->
    <el-breadcrumb separator-class="el-icon-arrow-right" class="nav">
      <el-breadcrumb-item>酒店</el-breadcrumb-item>
      <el-breadcrumb-item>南京市酒店预订</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 搜索栏 -->
    <HotelFilters/>
  </div>
</template>

<script>
import HotelFilters from '@/components/hotel/hotelFilters' 
export default {
  components: {
      HotelFilters
  }
};
</script>

<style lang="less" scoped>
.container{
  width:1000px;
  margin:0 auto;
  .nav{
      margin-top: 20px
  }
}
</style>

