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
        <h2 class="contentTitle">{{postDetailInfo.title}}</h2>
        <!-- 内容部分 -->
        <div class="reading">
          <span>阅读：{{postDetailInfo.watch}}</span>
          <span>攻略：{{postDetailInfo.updated_at|getMyDate}}</span>
        </div>
        <div v-html="postDetailInfo.content" class="detail"></div>
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
        <!-- 评论部分 -->
        <div class="comments">
          <h4>评论</h4>
          <el-tag :key="tagName" closable type="info">回复 @地球发动机</el-tag>
          <el-input type="textarea" v-model="commentContent" placeholder="请输入内容"></el-input>
          <el-row type="flex" justify="space-between" class="submitArea">
            <el-upload
              action="http://localhost:1337/upload"
              list-type="picture-card"
              :on-preview="handlePictureCardPreview"
              :on-remove="handleRemove"
              name="files"
              :on-success="handleAvatarSuccess"
              :before-upload="beforeAvatarUpload"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
            <!-- 图片展示区域 -->
            <!-- <el-dialog :visible.sync="dialogVisible" size="tiny">
  <img width="100%" :src="dialogImageUrl" alt="">
            </el-dialog>-->
            <el-button type="primary" width="216" height="28">提交</el-button>
          </el-row>
          <!-- 评论组件部分 -->
          <Comments :data="componentsData" />
        </div>
        <!-- 右侧推荐相关攻略部分 -->
      </el-col>
      <el-col class="rightPost">
        <div class="rightPostTitle">相关攻略</div>
        <el-row v-for="(item,index) in rightPostInfo" :key="index">
          <img :src="item.images[0]" alt />
          <span>
            <i>{{item.title}}</i>
            <em>{{item.updated_at|getMyDate}} 阅读 {{item.watch}}</em>
          </span>
        </el-row>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import Comments from "@/components/post/comments.vue";
import monent from "moment";
export default {
  components: {
    Comments
  },
  data() {
    return {
      tagName: "地球发动机",
      commentContent: "",
      id: 4,
      rightPostInfo: [
        // {updated_at:1558596111628}
      ],
      updated_at: 0,
      // 文章详情
      postDetailInfo: {
        content: null, //  文章内容
        title: "", //  文章标题
        updated_at: 0, // 更新时间
        watch: 0 //  文章阅读量
      },
      // 获取文章评论需要数据
      commentsAPIData: {
        post: 4, // 文章id
        // _sort: "Array", //  排序
        _limit: 2, //  条数
        _start: 0 //  开始数据（分页）
      },
      // 文章评论数据
      componentsData: [],
      // 提交评论相关数据
      submitCommentsData: {
        pics: [], //图片
        content: "", //  评论内容
        post: 4 // 文章id
      }
    };
  },
  filters: {
    // 过滤处理时间数据
    getMyDate(value) {
      return monent(value).format("YYYY-MM-DD HH:mm");
    }
  },
  methods: {
    // 上传文件成功时的钩子
    handleAvatarSuccess(res) {
      //   console.log(res);
      this.submitCommentsData.pics.push(res[0]);
      console.log(this.submitCommentsData.pics);
    },
    // 上传文件之前的钩子
    beforeAvatarUpload(files) {
      // console.log(files);
      if(files.type.indexOf("image/")==-1){
        this.$message.warning("请上传正确格式的图片")
        return false
      }
    },
    // 删除图片
    handleRemove(file, fileList) {
      console.log(file, fileList)
    },
    // 图片展示墙
    handlePictureCardPreview() {}
  },
  mounted() {
    // console.log(this.$store.state.user)
    this.$axios({
      url: "/posts",
      params: { id: this.id }
    }).then(res => {
      console.log(res.data);
      this.postDetailInfo = res.data.data[0];
    });
    // 获取推荐文章
    this.$axios({
      url: "/posts/recommend",
      params: { id: this.id }
    }).then(res => {
      // console.log(res.data);
      this.rightPostInfo = res.data.data;
    });
    // 获取文章评论数据
    this.$axios({
      url: "/posts/comments",
      params: this.commentsAPIData
    }).then(res => {
      const { data } = res.data;
      this.componentsData = data;
      console.log(this.componentsData);
    });
  }
};
</script>
<style lang="less" scoped>
#Postcontainer {
  width: 1000px !important;
  margin: 20px auto 0;
  .leftContent {
    max-width: 700px !important;
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
    .dataShare {
      padding: 50px 0 30px 0;
      text-align: center;
      span {
        cursor: pointer;
        display: inline-block;
        height: 56px;
        margin: 0 12px;
        i {
          display: block;
          height: 32px;
          font-size: 32px;
          color: #ffa500;
          margin-bottom: 0.2em;
        }
        em {
          font-size: 14px;
          color: #999;
        }
      }
    }
    .comments {
      margin-bottom: 50px;
      h4 {
        margin-bottom: 24px;
      }
      .el-tag {
        margin-bottom: 12px;
      }
      /deep/ .submitArea {
        margin: 12px 0 32px;
        .el-upload--picture-card {
          width: 100px;
          height: 100px;
          line-height: 100px;
        }
        .el-button--primary {
          width: 56px;
          height: 28px;
          text-align: center;
          line-height: 28px;
          position: relative;
          span {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            // margin:0 auto;
          }
        }
      }
    }
  }
  .rightPost {
    width: 280px !important;
    .el-row {
      height: 120px;
      padding: 20px 0;
      display: flex;
      border-bottom: 1px solid #ddd;
      img {
        width: 100px;
        height: 80px;
        background-color: #ddd;
        margin-right: 8px;
      }
      span {
        flex: 1;
        i {
          display: block;
          height: 3.9em;
          font-size: 16px;
          overflow: hidden;
        }
        em {
          font-size: 12px;
          color: #999;
        }
      }
    }
    .rightPostTitle {
      font-size: 18px;
      font-weight: 400;
      border-bottom: 1px solid #ddd;
      padding-bottom: 16px;
    }
  }
}
</style>


