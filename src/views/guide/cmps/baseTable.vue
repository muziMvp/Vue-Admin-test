<template>
  <div class="app-table">
    <el-table 
      ref="elTable" 
      :header-cell-style="{
        background: '#f4f4f4',
        'font-weight': '600',
        'border-right': 'none',
      }" 
      border 
      highlight-current-row 
      :cell-style="{
    'border-bottom': '1px solid #e5e5e5',
    'border-right': 'none',
  }" v-loading="loading" :element-loading-text="'正在加载.........'" :data="tableData" :row-key="rowKey"
  >
    <base-column v-for="(column, index) in columns" :key="`${column.key}_${index}`" :column="column" :tooltipShow='column.showTooltip'>
      <!--先把自定义配置项的作用域插槽内容(写在页面table组件里面的)映射到table组件内的具名插槽(在base-column组件内),然后base-column组件内又有对应的具名插槽,将最终值映射到base-column组件内具体展示 -->
      <template v-for="(slotItem, slotKey) in $scopedSlots" :slot="slotKey" slot-scope="{row,$index}">
        <slot v-if="!['optRow'].includes(slotKey)" :name="slotKey" :row="row" :$index="$index" :column="column">
        </slot>
      </template>
    </base-column>
    </el-table>
  </div>
</template>
<script>
import BaseColumn from './baseColumn'
export default {
  name: 'BaseTable',
  components: { BaseColumn },
  props: {
    // 表头配置
    columns: {
      type: Array,
      required: true
    },
    // 表头唯一标识
    rowKey: {
      type: String,
      default: 'id'
    },
    // 数据源
    dataList: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      loading: false,
      tableData: []
    }
  },
  created() {
    this.tableData=this.dataList;
  },
  methods: {}
}
</script>
<style lang="scss" scoped>
.app-table {
  border:1px solid red;
}
</style>
