<template>
    <div class="comments">
        <h4>评论</h4>
        <el-tag
          :key="tagName"
          closable
          type="info">
          回复 @地球发动机
        </el-tag>
        <el-input type="textarea" v-model="commentContent" placeholder="请输入内容"></el-input>
    </div>    
</template>
<script>
export default {
    data () {
        return {
            tagName:'地球发动机',
            commentContent:""
        }
    }
}
</script>
<style lang="less" scoped>
.comments{
    margin-bottom: 50px;
}
</style>


