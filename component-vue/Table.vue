<template>
  <div>
    <el-table
      :data="tableData"
      border
      style="width: 100%"
      :default-sort="defaultSort"
      :cell-style="cellStyle"
    >
      <template v-for="item in titleData">
        <template v-if="!item.__slotName">
          <el-table-column
            align="center"
            :key="item.prop"
            v-bind="item"
          >
          </el-table-column>
        </template>
        <!-- 插槽 __slotName-->
        <template v-if="item.__slotName">
          <el-table-column
            align="center"
            :key="item.prop"
            v-bind="item"
          >
            <template slot-scope="scope">
              <slot :name="item.__slotName" :row="scope.row"></slot>
            </template>
          </el-table-column>
        </template>
      </template>

      <!-- 操作列插槽 -->
      <template>
        <slot name="operation"></slot>
      </template>
    </el-table>
    <el-pagination
      v-if="isPagination"
      background
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="pageSizes"
      :page-size="pageSize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    >
    </el-pagination>
  </div>
</template>

<script>
export default {
  name: "HllTable",
  props: {
    // 表头数据
    titleData: {
      type: Array,
      default: () => [],
    },
    // 表格数据
    tableData: {
      type: Array,
      default: () => [],
    },
    
    // 表格默认排序
    defaultSort: {
      type: Object,
      default: function () {
        return {
          prop: "date",
          order: "descending",
        };
      },
    },

    // 是否开启分页
    isPagination: {
      type: Boolean,
      default: true,
    },

    // 当前是第几页
    currentPage: {
      type: Number,
      default: 1,
    },
    // 分页数据选项
    pageSizes: {
      type: Array,
      default: () => [10, 20, 50, 100],
    },
    // 每页显示的数据条数
    pageSize: {
      type: Number,
      default: 10,
    },
    // 数据总数
    total: {
      type: Number,
      default: 100,
    },
    cellStyle:{
      type:Function
    }
  },
  data() {
    return {
      url: "https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg",
      srcList: [],
    };
  },
  methods: {

    handleSizeChange(val) {
      this.$emit("sizeChange", val);
    },
    handleCurrentChange(val) {
      this.$emit("currentChange", val);
    },
  },
};
</script>
<style>
</style>