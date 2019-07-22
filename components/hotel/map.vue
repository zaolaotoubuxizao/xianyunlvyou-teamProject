<template>
  <div class="maps">
    <!-- 地图容器 -->
    <div id="container" style="width:650px; height: 360px;"></div>
    <!-- 右侧风景，交通显示 -->
    <div class="way">
      <div class="way_tab">
        <div class="sty_1" v-for="(item, index) in ['风景', '交通']" :key='index' @click='handleClick(index)' :class="{ sty_show: index === current }">{{item}}</div>
      </div>
      <div class="way_main">
        <!-- 风景 -->
        <ul>
          <li v-for='(item,index) in sceneryData' :key='index' v-if='current===0' ref='sceneryData'>
            <span>{{item.name}}</span>
            <span>{{item.km}}</span>
          </li>
        </ul>
        <!-- 交通 -->
        <ul v-for='(item,index) in wayData' :key='index' v-if='current===1'>
          <li>
            <span>{{item.name}}</span>
            <span>{{item.km}}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: 0,
      sceneryData: [
        {
          km: "13公里",
          name: "渡江胜利纪念馆1"
        },
        {
          km: "12公里",
          name: "渡江胜利纪念馆2"
        },
        {
          km: "12公里",
          name: "渡江胜利纪念馆3"
        },
        {
          km: "10公里",
          name: "渡江胜利纪念馆4"
        },
        {
          km: "19公里",
          name: "渡江胜利纪念馆5"
        },
        {
          km: "13公里",
          name: "渡江胜利纪念馆6"
        },
        {
          km: "13公里",
          name: "渡江胜利纪念馆7"
        },
        {
          km: "13公里",
          name: "渡江胜利纪念馆8"
        },
        {
          km: "13公里",
          name: "渡江胜利纪念馆9"
        },
        {
          km: "13公里",
          name: "渡江胜利纪念馆10"
        }
      ],
      wayData: [
        {
          km: "13公里",
          name: "农贸中心地铁站1"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站2"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站3"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站4"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站5"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站6"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站7"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站8"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站9"
        },
        {
          km: "13公里",
          name: "农贸中心地铁站10"
        }
      ]
    };
  },
  mounted() {
    // 等待页面加载完成之后才执行
    // container是页面的div容器
    window.onLoad = function() {
      var map = new AMap.Map("container", {
        zoom: 11, //级别
        center: [116.397428, 39.90923], //中心点坐标
        viewMode: "3D" //使用3D视图
      });

      // 创建一个 Marker 实例：
      var marker = new AMap.Marker({
        position: new AMap.LngLat(116.39, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
        icon: "//vdata.amap.com/icons/b18/1/2.png", // 添加 Icon 图标 URL
        title: "北京"
      });

      // 将创建的点标记添加到已有的地图实例：
      map.add(marker);
    };

    var key = "79c8365acf891602d1f6e3bd751cb27c";
    var url = `https://webapi.amap.com/maps?v=1.4.15&key=${key}&callback=onLoad`;

    var jsapi = document.createElement("script");
    jsapi.charset = "utf-8";
    jsapi.src = url;
    document.head.appendChild(jsapi);
  },
  methods: {
    handleClick(index) {
      this.current = index;
    }
  }
};
</script>

<style lang="less" scoped>
.maps {
  height: 360px;
  display: flex;
  #container {
    width: 650px;
  }
  .way {
    width: 350px;
    padding-left: 20px;
    .way_tab {
      height: 40px;
      margin-bottom: 20px;
      display: flex;
      font-size: 16px;
      border-bottom: 2px solid #e4e7ed;
      // 鼠标点击时添加的样式
      .sty_show {
        color: #84baf0;
        border-bottom: 2px solid #0094ff;
      }
      .sty_1 {
        margin-right: 40px;
      }
      .sty_1:hover {
        color: #409eff;
        cursor: pointer;
      }
    }
    .way_main {
      height: 300px;
      // 内容超过显示滚动条
      overflow: auto;
      li {
        height: 20px;
        color: #666;
        font-size: 16px;
        padding: 15px 0;
        span:nth-child(1) {
          display: inline-block;
          width: 230px;
        }
      }
    }
  }
}
</style>


