<template>
  <div style="height: 40px;border: 1px solid red;">
    {{item.value}}
  </div>
</template>

<script>
export default {
  props: {
    item: Object
  }
}
</script>
