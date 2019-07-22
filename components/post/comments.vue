<template>
  <div class="comments">
      <div v-for="(item,index) in data" :key="index">
          {{item.content}}
          <item v-if="item.length>0" :data="item"/>
      </div>
    <!-- <div
      style="padding-bottom:40px;line-height:40px;border:1px solid red"
      v-for="(item,index) in dataaa"
      :key="index"
    > -->
<!--       <div v-for="(value,index) in dataaa" :key="index">
          <div style="height:40px;line-height:40px;border:1px solid red" v-if="key==='content'">
        {{value}} -->

        <!-- <div style="height:40px;line-height:40px;border:1px solid yellow" v-for="(value,key,index) in item.parent" :key="index"> -->
          <!-- {{value}} -->
          <!-- <div >
          </div> -->
        <!-- </div> -->
<!--           </div>
          <Item :data="value"/>
      </div> -->
    <!-- </div> -->
  </div>
</template>
<script>
export default {
  props: ["data"],
  name: "Item",
  data() {
    return {
      tagName: "地球发动机",
      commentContent: "",
      // 提交评论相关数据
      submitCommentsData: {
        pics: [], //图片
        content: "", //  评论内容
        post: 4 // 文章id
      }
    };
  },
  mounted() {},
  methods: {

  }
};
</script>
<style lang="less" scoped>

</style>


