<template>
    <div class="container">
        <el-row class="comment">
            <el-col class="user_info">
                <!-- 头像部分 -->
                <img data-v-370f0998="" width="40" src="https://n4-q.mafengwo.net/s11/M00/64/81/wKgBEFsmE0WAPbqnAAAOwSUlOKY63.jpeg?imageMogr2%2Fthumbnail%2F%2196x96r%2Fgravity%2FCenter%2Fcrop%2F%2196x96%2Fquality%2F90" alt="">
                <!-- 等级 -->
                <span>lv8</span>
                <!-- 时间 -->
                <div>2019-6-26</div>
            </el-col>
            <!-- 评论部分 -->
            <el-col class="txt_input">
                <el-row class="user_name">badwoman</el-row>
                <div class="txt">
                    <textarea name="" id="" placeholder="添加回复" @blur='set_height' :class="{set_height:isShow}" @focus="set_height" v-on:blur="restore"></textarea>
                    <div class="line">
                        <el-button type="primary" class="btn" v-if='isShow'>回复</el-button>
                    </div>
                </div>
            </el-col>
        </el-row>
    </div>
</template>



<script>
export default {
  data() {
    return {
      isShow: false
    };
  },
  methods: {
    //   获取焦点
    set_height() {
      this.isShow = true;
    },
    // 失去焦点
    restore() {
      this.isShow = false;
    }
  }
};
</script>

<style lang="less" scoped>
.comment {
  display: flex;
  .user_info {
    width: 110px;
    img {
      border-radius: 50%;
      border: 2px solid #ffaa33;
    }
    span {
      color: #ffaa33;
      position: relative;
      top: -20px;
      left: 0;
    }
    div {
      font-size: 14px;
      color: #66686f;
    }
  }
  .txt_input {
    .user_name {
      height: 30px;
      color: #66686f;
    }
    .txt {
      width: 890px;
      height: 24px;
      box-sizing: border-box;

      .set_height {
        height: 100px;
        border: 1px solid #409eff;
      }
      textarea {
        width: 100%;
        height: 22px;
        line-height: 23px;
        resize: none;
        font-size: 14px;
        outline: none;
        padding: 5px 10px;
        overflow-y: hidden;
        border-radius: 5px;
      }
      .line {
        width: 915px;
        border-bottom: 1px dashed #ccc;
        padding: 5px 0;
        .btn {
          margin-left: 840px;
        }
      }
    }
  }
}
</style>


