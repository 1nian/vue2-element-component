<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item
        :to="item.path"
        v-for="(item, index) in linkList"
        :key="index"
      >
        <i v-if="index === 0" class="el-icon-house"></i
        >{{ item.title }}</el-breadcrumb-item
      >
    </el-breadcrumb>
  </div>
</template>
<script>
export default {
  props: {
    linkList: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style>
</style>
