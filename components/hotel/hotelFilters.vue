<template>
  <div style="height:1000px">
    <el-row type="flex" justify="space-between" class="nav">
      <el-col>
        <el-autocomplete
          v-model="city"
          :fetch-suggestions="querySearchAsync"
          placeholder="请输入内容"
          @select="handleSelect"
        ></el-autocomplete>
      </el-col>
      <el-col>
        <div class="block">
          <el-date-picker
            v-model="time"
            type="daterange"
            start-placeholder="入住日期"
            end-placeholder="离店日期"
            default-value="2010-10-01"
          ></el-date-picker>
        </div>
      </el-col>

      <el-col class="number">
        <input
          type="text"
          readonly="readonly"
          autocomplete="off"
          placeholder="人数未定"
          class="el-input__inner"
          @click="isShow=!isShow"
        />
      </el-col>

      <el-col>
        <el-button type="primary">查看价格</el-button>
      </el-col>
    </el-row>
    <el-form ref="form" :model="form" label-width="30px" class="yinc" v-show="isShow">
      <el-form-item>
        <span>每间</span>
        <el-select v-model="form.adult" placeholder="2成人" style="width:100px">
          <el-option v-for="(item,index) in adult" :key="index" :label="`${item}成人`" :value="item"></el-option>
        </el-select>
        <el-select v-model="form.child" placeholder="0儿童" style="width:100px">
          <el-option v-for="(item,index) in child" :key="index" :label="`${item}儿童`" :value="item"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary">确定</el-button>
      </el-form-item>
    </el-form>
    <div class="tu">
      <el-row type="flex">
        <el-col :span="14">
          <el-row type="flex">
            <el-col :span="4">区域：</el-col>
            <el-col :span="20">
              <nuxt-link
                to="/"
                v-for="(item,index) in cities.scenics"
                :key="index"
                class="spot"
              >{{item.name}}</nuxt-link>
            </el-col>
          </el-row>
          <el-row type="flex">
            <el-col :span="4">攻略：</el-col>
            <el-col
              :span="20"
            >北京，你想要的都能在这找到。博古通今，兼容并蓄，天下一城，如是帝都。 景点以故宫为中心向四处辐射；地铁便宜通畅，而且覆盖绝大多数景点。 由于早上有天安门升旗仪式，所以大多数人选择在天安门附近住宿。</el-col>
          </el-row>
          <el-row type="flex">
            <el-col :span="4">
              均价
              <el-tooltip content="等级均价由平日价格计算得出，节假日价格会有上浮。" placement="top-start">
                <sup class="question-mark">?</sup>
              </el-tooltip>:
            </el-col>
            <el-col :span="20">
              <el-tooltip
                content="等级均价由平日价格计算得出，节假日价格会有上浮。"
                placement="bottom-start"
                :visible-arrow="false"
              >
                <sup class="xing">
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i> ￥332
                </sup>
              </el-tooltip>
              <el-tooltip
                content="等级均价由平日价格计算得出，节假日价格会有上浮。"
                placement="bottom-start"
                :visible-arrow="false"
              >
                <sup class="xing">
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i> ￥521
                </sup>
              </el-tooltip>
              <el-tooltip
                content="等级均价由平日价格计算得出，节假日价格会有上浮。"
                placement="bottom-start"
                :visible-arrow="false"
              >
                <sup class="xing">
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i>
                  <i class="iconfont iconhuangguan"></i> ￥768
                </sup>
              </el-tooltip>
            </el-col>
          </el-row>
        </el-col>
        <el-col :span="10">
          <div id="container" style="width:100%; height: 100%;"></div>
        </el-col>
      </el-row>
    </div>
    <div class="huo">
      <el-row type="flex">
        <el-col :span="6">
          <div class="block">
            <el-row>
              <span>价格</span>
              <span>0-3300</span>
            </el-row>
            <el-row>
              <el-slider v-model="moeny" :format-tooltip="formatTooltip"></el-slider>
            </el-row>
          </div>
        </el-col>
        <el-col :span="5">
          <el-row>住宿等级</el-row>
          <el-row>
            <el-select
              v-model="condition.levels"
              multiple
              collapse-tags
              style="margin-left: 20px;"
              placeholder="不限"
            >
              <el-option
                v-for="item in conditions.levels"
                :key="item.id"
                :label="item.name"
                :value="item.level"
              ></el-option>
            </el-select>
          </el-row>
        </el-col>
        <el-col :span="5">
          <el-row>住宿类型</el-row>
          <el-row>
            <el-select
              v-model="condition.types"
              multiple
              collapse-tags
              style="margin-left: 20px;"
              placeholder="不限"
            >
              <el-option
                v-for="item in conditions.types"
                :key="item.id"
                :label="item.name"
                :value="item.name"
              ></el-option>
            </el-select>
          </el-row>
        </el-col>
        <el-col :span="4">
          <el-row>酒店设施</el-row>
          <el-row>
            <el-select
              v-model="condition.assets"
              multiple
              collapse-tags
              style="margin-left: 20px;"
              placeholder="不限"
            >
              <el-option
                v-for="item in conditions.assets"
                :key="item.id"
                :label="item.name"
                :value="item.name"
              ></el-option>
            </el-select>
          </el-row>
        </el-col>
        <el-col :span="4">
          <el-row>酒店品牌</el-row>
          <el-row>
            <el-select
              v-model="condition.brands"
              multiple
              collapse-tags
              style="margin-left: 20px;"
              placeholder="不限"
            >
              <el-option
                v-for="item in conditions.brands"
                :key="item.id"
                :label="item.name"
                :value="item.name"
              ></el-option>
            </el-select>
          </el-row>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: "",
      time: "",
      options: [{ value: "1" }, { value: "2" }],
      adult: [1, 2, 3, 4, 5, 6, 7],
      child: [0, 1, 2, 3, 4],
      form: {
        adult: "",
        child: ""
      },
      isShow: false,
      cities: [],
      moeny: 50,
      conditions: {},
      condition: {
        levels: [], // 酒店等级
        types: [], // 酒店类型
        assets: [], // 酒店设施
        brands: [] // 酒店品牌
      },
      options: [
        {
          value: "选项1",
          label: "黄金糕"
        },
        {
          value: "选项2",
          label: "双皮奶"
        },
        {
          value: "选项3",
          label: "蚵仔煎"
        },
        {
          value: "选项4",
          label: "龙须面"
        },
        {
          value: "选项5",
          label: "北京烤鸭"
        }
      ],
      value11: []
    };
  },
  mounted() {
    this.$axios({
      url: "/hotels"
    }).then(res => {
      console.log(res);
    });
    this.$axios({
      url: "/hotels/options"
    }).then(res => {
      this.conditions = res.data.data;
      console.log(this.conditions);
    });
    this.$axios({
      url: "/cities",
      params: {
        name: "南京市"
      }
    }).then(res => {
      // console.log(res.data.data[0]);
      this.cities = res.data.data[0];
    });
    window.onLoad = function() {
      // 等待页面加载完成之后才执行
      // container是页面的div容器
      var map = new AMap.Map("container", {
        zoom: 11, //级别
        center: [118.8718107, 31.32846821], //中心点坐标
        viewMode: "3D" //使用3D视图
      });

      // 创建一个 Marker 实例：
      var marker1 = new AMap.Marker({
        content: `<div style="width:20px; height:20px; background: red; text-align: center; line-height:20px">1</div>`,
        position: new AMap.LngLat(118.8618107, 31.33846821), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
        title: "北京"
      });

      var marker2 = new AMap.Marker({
        position: new AMap.LngLat(118.8718117, 31.32846811), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
        title: "北京"
      });

      // 将创建的点标记添加到已有的地图实例：
      map.add([marker1, marker2]);
    };

    var key = "2c047e1737fe327f243ea9bd2d627c3c";
    var url = `https://webapi.amap.com/maps?v=1.4.15&key=${key}&callback=onLoad`;

    var jsapi = document.createElement("script");
    jsapi.charset = "utf-8";
    jsapi.src = url;
    document.head.appendChild(jsapi);
  },
  methods: {
    querySearchCity(queryString) {
      return new Promise((resolve, reject) => {
        if (!queryString) {
          resolve([]); // 不出现加载下拉框
          return;
        }

        // 请求推荐城市列表
        this.$axios({
          url: "/cities",
          params: {
            name: queryString
          }
        }).then(res => {
          // 推荐列表的数组
          const { data } = res.data;

          // 给数组中的对象添加value
          const newData = data.map(v => {
            v.value = v.name;
            return v;
          });

          this.city = newData[0].value;
          resolve(newData);
        });
      });
    },
    async querySearchAsync(value, cb) {
      const res = await this.querySearchCity(value);

      // if (res.length > 0) {
      //   this.form.departCity = res[0].value;
      //   this.form.departCode = res[0].sort;
      // }

      cb(res);
    },
    handleSelect(item) {},
    formatTooltip(val) {
      return val * 40;
    }
  }
};
</script>

<style lang="less" scoped>
.nav {
  margin-top: 10px;
}
.el-col {
  margin-right: 10px;
}
.number {
  position: relative;
}
.yinc {
  border: 1px solid #ebebeb;
  border-radius: 3px;
  transition: 0.2s;
  width: 300px;
  padding-top: 10px;
  position: absolute;
  top: 150px;
  left: 832px;
  z-index: 999;
  .el-button {
    float: right;
    margin-right: 10px;
  }
}
.spot {
  margin-right: 5px;
}
.question-mark {
  display: inline-block;
  width: 15px;
  height: 15px;
  border-radius: 50%;
  font-size: 12px;
  text-align: center;
  background-color: #ccc;
  color: #fff;
}
.xing {
  margin-right: 20px;
  font-size: 14px;
  i {
    color: #f90;
    letter-spacing: -5px;
  }
}
.tu {
  .el-row {
    margin-top: 10px;
  }
}
.huo {
  margin-right: 10px;
  margin-top: 10px;
  border: 1px solid #ccc;
  .el-row {
    margin: 5px;
  }
  .el-col {
    border-right: 1px solid #ccc;
  }
}
</style>

