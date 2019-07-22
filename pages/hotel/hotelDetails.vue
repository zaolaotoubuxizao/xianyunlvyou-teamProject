<!--酒店详情页面-->
<template>
  <div class="container">
    <!-- 面包屑 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">酒店</el-breadcrumb-item>
      <el-breadcrumb-item :to="{ path: '/' }">南京酒店</el-breadcrumb-item>
      <el-breadcrumb-item>好来阁商务宾馆</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 宾馆标题部分 -->
    <el-row class="title">
      <span class="title_txt">好来阁商务宾馆</span>
      <span class="tiele_xingxing">
        <i class="iconfont iconhuangguan"></i>
        <i class="iconfont iconhuangguan"></i>
        <i class="iconfont iconhuangguan"></i>
        <i class="iconfont iconhuangguan"></i>
        <i class="iconfont iconhuangguan"></i>
      </span>
    </el-row>

    <el-row class="eg">hao lai ge shang wu hotel</el-row>
    <el-row class="position_txt">
      <i class="iconfont iconlocation"></i>
      <el-col>高淳县淳溪镇镇兴路118号(高淳县委党校对面)</el-col>
    </el-row>

    <!-- 宾馆图片部分 -->
    <el-row class="hotel_img">
      <!-- 图片展示 -->
      <el-col class="show_img">
        <img :src="default_img" alt="好来阁商宾馆" width="640" height="360">
      </el-col>
      <!-- 图片数据 -->
      <el-col class="data_img">
        <img :src="`http://157.122.54.189:9093${item.img}`" alt="好来阁商宾馆" v-for='(item,index) in data_img' :key='index' @click='clk(index)' ref='dataImg'>
      </el-col>
    </el-row>

    <!-- 宾馆价格来源部分 -->

    <el-table :data="products" stripe style="width: 100%" class="priceFrom">
      <el-table-column prop="name" label="价格来源" width="420">
      </el-table-column>
      <el-table-column prop="bestType" label="低价房型" width="420">
      </el-table-column>
      <el-table-column prop="price" label="最低价格/每晚">
      </el-table-column>
    </el-table>

    <!-- 地图组件 -->
    <div class="map">
      <Map/>
    </div>

    <!-- 酒店基本信息栏 -->
    <el-row class="hotel_information">
      <el-col>基本信息</el-col>
      <el-col class="jl">入住时间：14:00之后</el-col>
      <el-col>离店时间：12:00之前</el-col>
      <el-col>2011年开业/2011年装修</el-col>
      <el-col>酒店规模：63间客房</el-col>
    </el-row>
    <el-row class="hotel_installation">
      <el-col :span='5'>主要设施</el-col>
      <el-col :span='19'>
        <span class="font_sty">wifi</span>
      </el-col>
    </el-row>
    <el-row class="hotel_park">
      <el-col :span='5'>停车服务</el-col>
      <el-col :span='19'>免费;提供免费停车位;酒店停车场，免费</el-col>
    </el-row>
    <el-row class="hotel_brand">
      <el-col :span='5'>品牌信息</el-col>
      <el-col :span='19'>希尔顿</el-col>
    </el-row>

    <!-- 用户评论展示 -->
    <el-row class="comment_title">
      <h3>10条真实用户评论</h3>
    </el-row>
    <el-row class="commit_main">
      <el-col :span='5'>
        <el-row>总评数：161</el-row>
        <el-row>好评数：35</el-row>
        <el-row>差评数：5</el-row>
      </el-col>
      <el-col class="commit_main_img" :span='19'>
        <el-col>
          <el-progress type="circle" :percentage="100" :show-text='false' color='#ffe6c0' width='74' stroke-width='2'></el-progress>
          <el-col class="circle_text1">
            <el-row>推荐</el-row>
          </el-col>
          <el-col class="grade">
            <el-rate v-model="value" disabled show-score text-color="#ff9900" score-template="{value}">
            </el-rate>
          </el-col>
        </el-col>
        <el-col>
          <el-progress type="circle" :percentage="75" :show-text='false' color='#ff9900' width='74' stroke-width='2'></el-progress>
          <el-col class="circle_text2">
            <el-row>环境</el-row>
            <el-row>7.3</el-row>
          </el-col>
        </el-col>
        <el-col>
          <el-progress type="circle" :percentage="75" :show-text='false' color='#ff9900' width='74' stroke-width='2'></el-progress>
          <el-col class="circle_text3">
            <el-row>环境</el-row>
            <el-row>7.3</el-row>
          </el-col>
        </el-col>
        <el-col>
          <el-progress type="circle" :percentage="75" :show-text='false' color='#ff9900' width='74' stroke-width='2'></el-progress>
          <el-col class="circle_text4">
            <el-row>环境</el-row>
            <el-row>7.3</el-row>
          </el-col>
        </el-col>
      </el-col>
    </el-row>

    <!-- 评论组件 -->
    <div class="remark">
      评论部分
    </div>

  </div>
</template>

<script>
import Map from "@/components/hotel/map";
export default {
  components: {
    Map
  },
  data() {
    return {
      // 只读星星评分
      value: 3.5,
      // 宾馆价格来源数据源
      products: [
        {
          name: "携程",
          bestType: "高级大床房A",
          price: "524"
        },
        {
          name: "携程",
          price: "524",
          bestType: "高级大床房A"
        },
        {
          name: "携程",
          price: "524",
          bestType: "高级大床房A"
        }
      ],
      // 默认展示的图片
      default_img: " http://157.122.54.189:9093/images/hotel-pics/1.jpeg",
      data_img: [
        {
          img: "/images/hotel-pics/1.jpeg"
        },
        {
          img: "/images/hotel-pics/2.jpeg"
        },
        {
          img: "/images/hotel-pics/3.jpeg"
        },
        {
          img: "/images/hotel-pics/4.jpeg"
        },
        {
          img: "/images/hotel-pics/5.jpeg"
        },
        {
          img: "/images/hotel-pics/6.jpeg"
        }
      ]
    };
  },
  methods: {
    //   点击小图。把小图的链接传给大图
    clk(index) {
      this.default_img = this.$refs.dataImg[index].src;
    }
  }
};
</script>

<style lang="less" scoped>
.container {
  width: 1000px;
  margin: 0 auto;
  margin-top: 20px;
}
.title {
  margin-top: 25px;
  .title_txt {
    color: #333;
    font-weight: 400;
    font-size: x-large;
  }

  .iconhuangguan {
    color: #f90;
  }
}
.eg {
  color: #666666;
}
.position_txt {
  display: flex;
  color: #666666;
  margin-bottom: 35px;

  .iconlocation {
    display: block;
    margin-top: 2px;
  }
}

.hotel_img {
  box-sizing: border-box;
  .show_img {
    width: 640px;
    height: 360px;
    margin-right: 26px;
    background-color: pink;
  }
  .data_img {
    display: block;
    width: 334px;
    height: 360px;
    img {
      width: 160px;
      height: 110px;
      padding-right: 7px;
      padding-bottom: 10px;
    }
  }
}
.priceFrom {
  margin-top: 60px;
}
.map {
  height: 360px;
  margin-top: 30px;
  margin-bottom: 50px;
}
.hotel_information {
  display: flex;
  height: 59px;
  border-bottom: 1px solid #ccc;
  line-height: 59px;
  .jl {
    margin-left: 10px;
  }
}
.hotel_installation {
  display: flex;
  height: 59px;
  border-bottom: 1px solid #ccc;
  line-height: 59px;
  .font_sty {
    background-color: #eee;
    padding: 5px 10px 5px 10px;
  }
}
.hotel_park {
  display: flex;
  height: 59px;
  border-bottom: 1px solid #ccc;
  line-height: 59px;
}
.hotel_brand {
  display: flex;
  height: 59px;
  border-bottom: 1px solid #ccc;
  line-height: 59px;
}
.comment_title {
  margin-top: 30px;
}
.commit_main {
  display: flex;
  height: 114px;
  padding: 20px;
  .commit_main_img {
    display: flex;
    position: relative;
    .circle_text1 {
      width: 60px;
      height: 60px;
      color: #ffe6c0;
      font-size: 20px;
      background-color: #fff;
      transform: rotate(-30deg);
      position: absolute;
      z-index: -5;
      top: 18px;
      left: 20px;
    }
    .circle_text2 {
      width: 60px;
      height: 60px;
      background-color: #fff;
      color: #ff9900;
      position: absolute;
      z-index: -5;
      top: 20px;
      left: 215px;
    }
    .circle_text3 {
      width: 60px;
      height: 60px;
      background-color: #fff;
      position: absolute;
      color: #ff9900;
      z-index: -5;
      top: 20px;
      left: 400px;
    }
    .circle_text4 {
      width: 60px;
      height: 60px;
      color: #ff9900;
      background-color: #fff;
      position: absolute;
      z-index: -5;
      top: 20px;
      left: 595px;
    }
    .grade {
      position: absolute;
      top: 25px;
      left: -24px;
    }
  }
}

.remark {
  background-color: orange;
}
</style>


