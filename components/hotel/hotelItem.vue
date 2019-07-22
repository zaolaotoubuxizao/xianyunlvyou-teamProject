<template>
  <div style="height:200px;padding:20px 0;border-bottom:1px solid #ccc">
    <el-row type="flex">
      <el-col :span="7">
        <nuxt-link :to="`/hotel/hotelDetails/id=${data.id}`">
          <img :src="data.photos" alt style="width:280px;height:210px;" />
        </nuxt-link>
      </el-col>
      <el-col :span="10" style="margin-left:10px;padding:0 10px;">
        <h2>
          <nuxt-link :to="`/hotel/hotelDetails/id=${data.id}`">{{data.name}}</nuxt-link>
        </h2>
        <div style="font-size:14px;">
          {{data.alias}}
          <span v-if="data.hotellevel">
            <span v-if="data.hotellevel.level == 1">
              <i class="iconfont iconhuangguan"></i>
            </span>
            <span v-if="data.hotellevel.level == 2">
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
            </span>
            <span v-if="data.hotellevel.level == 3">
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
            </span>
            <span v-if="data.hotellevel.level == 4">
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
            </span>
            <span v-if="data.hotellevel.level == 5">
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
              <i class="iconfont iconhuangguan"></i>
            </span>
          </span>
          <div style="display:inline;" v-if="data.hoteltype">{{data.hoteltype.name}}</div>
        </div>
        <el-row type="flex" style="margin-top:10px;">
          <el-col :span="10">
            <el-rate
              v-model="data.stars"
              disabled
              show-score
              text-color="#ff9900"
              score-template="{value}"
            ></el-rate>
          </el-col>
          <el-col :span="7">
            <span>78</span> 条评论
          </el-col>
          <el-col :span="7">
            <span>32</span> 篇游记
          </el-col>
        </el-row>
        <div style="margin-top:10px;">
          <i class="iconfont iconlocation"></i>
          位于：{{data.address}}
        </div>
      </el-col>
      <el-col :span="7">
        <el-row v-for="(item,index) in data.products" :key="index">
          <a href="https://hotels.ctrip.com/hotel/694679.html">
            <div
              style="border-bottom:1px solid #ccc;clear:both;height:50px;line-height:50px;padding-left:15px;"
              class="xc"
            >
              <div style="float:left;">{{item.name}}</div>
              <div style="float:right;margin-right:10px;">
                <span>￥{{item.price}}</span>起
                <i class="el-icon-arrow-right"></i>
              </div>
            </div>
          </a>
        </el-row>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  props: {
    data: {
      type: Object,
      default: {}
    }
  },
  mounted() {}
};
</script>

<style lang="less" scoped>
span {
  color: #f90;
}
.xc:hover {
  background-color: #eee;
}
</style>

