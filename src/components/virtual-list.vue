<template>
  <div class="viewport" ref="viewport" @scroll="handleScroll">
    <!-- 滚动条 -->
    <div class="scroll-bar" ref="scrollBar"></div>
    <!-- 真实渲染的内容 -->
    <div class="scroll-list" :style="{transform: `translate3d(0, ${offset}px, 0)`}">
      <div v-for="item in visibleData" :vid="item.id" :key="item.id">
        <slot :item="item"></slot>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    size: Number,
    remain: Number,
    items: Array
  },
  data() {
    return {
      start: 0,
      end: this.remain, // 默认显示8个
      offset: 0
    }
  },
  computed: {
    // 渲染三瓶
    // 前面预留几个
    prevCount() {
      return Math.min(this.start, this.remain);
    },
    // 后面预留几个
    nextCount() {
      return Math.min(this.remain, this.items.length - this.end);
    },
    // 可见的数据有哪些
    visibleData() {
      let start = this.start - this.prevCount;
      let end = this.end + this.nextCount;
      return this.items.slice(start, end);
    }
  },
  mounted() {
    this.$refs.scrollBar.style.height = this.size * this.items.length + 'px';
    this.$refs.viewport.style.height = this.size * this.remain + 'px';
  },
  methods: {
    handleScroll() {
      // 1. 当前滚过去几个了，当前应该显示第几个
      let scrollTop = this.$refs.viewport.scrollTop;
      // 计算滚过去几个
      this.start = Math.floor(scrollTop / this.size);
      this.end = this.start + this.remain; // 当前可渲染的区域
      // 定位当前的可视区域
      // 如果有预留渲染，应该把这个位置再向上移动当前这么多
      this.offset = this.start * this.size - this.size * this.prevCount;
    }
  }
}
</script>

<style scoped lang="scss">
  .viewport {
    overflow-y: scroll;
    position: relative;
  }
  .scroll-list {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
  }
</style>
