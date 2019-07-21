<template>
  <div id="Postcontainer">
    <el-row type="flex" justify="space-between">
      <el-col class="leftContent">
        <!-- 面包屑导航栏部分 -->
        <el-breadcrumb separator="/">
          <el-breadcrumb-item :to="{ path: '/post' }">旅游攻略</el-breadcrumb-item>
          <el-breadcrumb-item>攻略详情</el-breadcrumb-item>
        </el-breadcrumb>
        <!-- 标题部分 -->
        <h2 class="contentTitle">塞班贵？一定是你的打开方式不对！6000块玩转塞班</h2>
        <!-- 内容部分 -->
        <div class="reading">
          <span>阅读：2926</span>
          <span>攻略：2019-05-22</span>
        </div>
        <div v-html="content" class="detail"></div>
        <div class="dataShare">
          <span>
            <i class="el-icon-edit-outline"></i>
            <em>评论(100)</em>
          </span>
          <span>
            <i class="el-icon-star-off"></i>
            <em>收藏</em>
          </span>
          <span>
            <i class="el-icon-share"></i>
            <em>分享</em>
          </span>
          <span>
            <i class="iconfont iconding"></i>
            <em>点赞(31)</em>
          </span>
        </div>
        <!-- 评论组件部分 -->
        <Comments/>
        <!-- 右侧推荐相关攻略部分 -->
      </el-col>
      <el-col class="rightPost" @click.native="dwx">
        <div class="rightPostTitle">相关攻略</div>
        <el-row v-for="(item,index) in rightPostInfo" :key="index">
          <img :src="item.images[0]" alt="">
          <span>
            <i>{{item.title}}</i>
            <em>{{item.updated_at|getMyDate}} 阅读{{item.watch}}</em>
          </span>
        </el-row>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import Comments from "@/components/post/comments.vue"
import monent from "moment"
export default {
  components: {
    Comments
  },
  data() {
    return {
      id: 4,
      content: null,
      rightPostInfo:[
        // {updated_at:1558596111628}
      ],
      updated_at:0,
    };
  },
  filters: {
    // 过滤处理时间数据
    getMyDate(value){
      return monent(value).format("YYYY-MM-DD HH:mm:ss")
    }
  },
  methods:{
		dwx(){
      this.updated_at=this.rightPostInfo[0].updated_at
      // console.log(monent())
      console.log(this.updated_at)

    }
  },
  mounted() {
    // console.log(this.$store.state.user)
    this.$axios({
      url: "/posts",
      params: { id: this.id }
    }).then(res => {
      // console.log(res.data);
      this.content = res.data.data[0].content;
    });
    // 获取推荐文章
    setTimeout(()=>{
      this.$axios({
        url:'/posts/recommend',
        params: { id: this.id }
      })
        .then(res=>{
          console.log(res.data)
          this.rightPostInfo=res.data.data
        })
    },1)
  }
};
</script>
<style lang="less" scoped>
#Postcontainer {
  width: 1000px !important;
  margin: 20px auto 0;
  .leftContent {
    max-width: 700px!important;
    .contentTitle {
      margin-top: 20px;
      padding-bottom: 20px;
      font-size: 2em;
      border-bottom: 1px solid #ddd;
    }
    .reading {
      height: 64px;
      line-height: 64px;
      color: #999;
      span {
        float: right;
        margin-left: 20px;
      }
    }
    .detail {
     /deep/ p {
        margin-bottom: 2px;
        span {
          img {
              width: 100% !important;
              height: 100% !important;
              margin: 12px 0;
          } 
        }
      }
    }
    .dataShare{
      padding: 50px 0 30px 0;
      text-align: center;
      span{
        display: inline-block;
        height: 56px;
        margin: 0 12px;
        i{
          display: block;
          height: 32px;
          font-size: 32px;
          color: #ffa500;
        }
        em{
          font-size: 14px;
          color: #999;
        }
      }
    }
  }
  .rightPost {
    width: 280px !important; 
    .el-row{
      height: 120px;
      padding: 20px 0;
      display: flex;
      border-bottom: 1px solid #ddd;
      img{
        width: 100px;
        height: 80px;
        background-color: #ddd;
        margin-right: 8px;
      }
      span{
        flex: 1;
        i{
          display: block;
          height: 3.6em;
          font-size: 16px;
        }
        em{
          font-size: 12px;
          color: #999;
        }
      }
    }
    .rightPostTitle{
      font-size: 18px;
      font-weight: 400;
      border-bottom: 1px solid #ddd;
      padding-bottom: 16px;
    }                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
  }
}
</style>


