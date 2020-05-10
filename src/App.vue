<template>
  <div id="app">
    <!-- 告诉我的列表每一项多高，因为我要算出一个滚动条来 -->
    <VirtualList :size="40" :remain="8" :items="items">
      <Item slot-scope="{item}" :item="item"></Item>
    </VirtualList>
  </div>
</template>

<script>
import VirtualList from './components/virtual-list.vue';
import Item from './components/item.vue';
let items = [];
for(let i=0; i<100; i++) {
  items.push({id: i, value: i});
}

export default {
  name: 'App',
  data() {
    return {
      items
    }
  },
  components: {
    VirtualList,
    Item
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
