<template>
  <div class="container">
    <!-- 面包屑 -->
    <el-breadcrumb separator-class="el-icon-arrow-right" class="nav">
      <el-breadcrumb-item>酒店</el-breadcrumb-item>
      <el-breadcrumb-item>{{name}}酒店预订</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 搜索栏 -->
    <HotelFilters @changeinfo="changeinfo" />
    <!-- 酒店列表 -->
    <HotelItem v-for="(item,index) in newList" :key="index" :data="item" />
    <!-- 无数据是显示 -->
    <div
      v-if="newList.length == 0"
      style="text-align:center;height:100px;line-height:100px;"
    >暂无符合条件的酒店</div>
    <!-- 分页 -->
    <div class="block" style="height:80px;" v-if="newList.length != 0">
      <el-pagination
        layout="prev, pager, next"
        :page-size="pagesize"
        :current-page="page"
        @current-change="change"
        :total="total"
        style="float:right;margin-top:20px;"
      ></el-pagination>
    </div>
  </div>
</template>

<script>
import HotelFilters from "@/components/hotel/hotelFilters";
import HotelItem from "@/components/hotel/hotelItem";
export default {
  components: {
    HotelFilters,
    HotelItem
  },
  data() {
    return {
      total: 0,
      page: 1,
      pagesize: 2,
      info: {
        city: "74",
        // enterTime:'2019-06-12',
        // leftTime:'2019-07-15',
        _limit: "10",
        _start: "0"
      },
      name:'南京市',
      dataList: [],
      newList: [],
      oldList: [],
      hotellevel: {
        level: 0
      }
    };
  },
  mounted() {
    this.$axios({
      url: "/hotels",
      params: this.info
    }).then(res => {
      this.dataList = res.data.data;
      // this.dataList.forEach(v => {
      //   if (!v.hotellevel) {
      //     v.hotellevel = this.hotellevel;
      //   }
      // });
      this.newList = [...this.dataList].slice(
        (this.page - 1) * this.pagesize,
        this.page * this.pagesize
      );
      this.total = this.dataList.length;
    });
  },
  methods: {
    change(index) {
      this.page = index;
      this.oldList = [...this.dataList];
      this.newList = this.oldList.slice(
        (this.page - 1) * this.pagesize,
        this.page * this.pagesize
      );
      console.log(this.newList);
    },
    changeinfo(fo){
      this.info.city = fo.id;
      this.name = fo.city;
      this.$axios({
      url: "/hotels",
      params: this.info
    }).then(res => {
      this.dataList = res.data.data;
      // this.dataList.forEach(v => {
      //   if (!v.hotellevel) {
      //     v.hotellevel = this.hotellevel;
      //   }
      // });
      this.newList = [...this.dataList].slice(
        (this.page - 1) * this.pagesize,
        this.page * this.pagesize
      );
      this.total = this.dataList.length;
    });
    }
  }
};
</script>

<style lang="less" scoped>
.container {
  width: 1000px;
  margin: 0 auto;
  .nav {
    margin-top: 20px;
  }
}
</style>

