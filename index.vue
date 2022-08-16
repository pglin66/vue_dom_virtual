<!--
 * @Author: pglin66@126.com
 * @Date: 2022-08-15 17:13:44
 * @LastEditors: pglin66@126.com
 * @LastEditTime: 2022-08-16 11:08:15
 * @FilePath: 
 * @Description: 
-->
<template>
  <!-- <el-drawer title="询价详情" :visible.sync="drawer" :with-header="false" size="800px"> -->
  <div style="padding: 20px;">
    <div class="list-wrap" ref="listWrap" @scroll="scrollListener">
      <!-- 虚拟列表所有项的高度 -->
      <!-- <div class="scroll-bar" ref="scrollBar"></div> -->
      <!-- 虚拟列表项 -->
      <div :style="`height:${(list.length*30)}px`">
        <ul class="list" ref="list">
          <li v-for="val in showList">
            {{val}}
          </li>
        </ul>
      </div>

    </div>
  </div>
  <!-- </el-drawer> -->
</template>
<script>
export default {
  data () {
    return {
      drawer: true,
      info: null,

      list: [],// 超长的显示数据
      itemHeight: 30,// 每一列的高度
      showNum: 20,// 显示几条数据
      start: 0,// 滚动过程显示的开始索引
      end: this.showNum,// 滚动过程显示的结束索引
    }
  },
  computed: {
    // 计算可视窗口内要显示的数组 从长数组中截取要展示的头尾
    showList () {
      return this.list.slice(this.start, this.end);
    }
  },
  mounted () {
    // 列表项
    for (let i = 0; i < 111160; i++) {
      this.list.push('第' + i + '个')
    }
    // 计算滚动容器高度  每一项高度 * 可视窗口要展示的数据条数
    this.$refs.listWrap.style.height = this.itemHeight * this.showNum + 'px';
    // 计算总的数据需要的高度，构造滚动条高度   每一项高度 * 所有项
    // this.$refs.scrollBar.style.height = this.itemHeight * this.list.length + 'px';
  },

  methods: {
    scrollListener () {
      // 获取滚动卷去的高度
      let scrollTop = this.$refs.listWrap.scrollTop;
      // 开始的数组索引  卷去高度 / 每一项高度
      this.start = Math.floor(scrollTop / this.itemHeight);
      // 结束索引  开始索引 + 可视窗口内展示的数据条数
      this.end = this.start + this.showNum;
      // 动态计算卷去总高度给 虚拟列表一个顶部的偏移量，让数据一直保持在可视窗口中
      this.$refs.list.style.transform = 'translateY(' + this.start * this.itemHeight + 'px)';
      //transform: translateY(2805px);
    },
    open (info) {
      this.info = info;
      this.drawer = true
    }
  }
}
</script>
<style>
.list-wrap {
  position: relative;
  overflow-y: scroll;
  width: 200px;
  margin: 100px auto;
  height: 300px;
  box-sizing: border-box;
  border: solid 1px;
}
.list {
  margin: 0;
  padding: 0;
}
.list li {
  border: solid 1px;
  line-height: 30px;
}
</style>