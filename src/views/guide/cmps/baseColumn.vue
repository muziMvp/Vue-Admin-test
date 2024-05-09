<template>
  <el-table-column v-if="column.show != false" :prop="column.key" :label="column.label" :width="column.width"
    :align="column.align" :fixed="column.fixed" :sortable="column.sortable" :class-name="column.class"
    :show-overflow-tooltip="tooltipShow">
    <template #default="{ row, $index }">
      <render-dom v-if="column.render" :render-func="column.render" :row="row"></render-dom>
      <slot v-else-if="column.slotName" :name="column.slotName" :row="row" :$index="$index" :column="column"></slot>
      <span v-else>{{
        row[column.key] || row[column.key] === 0 ? row[column.key] : ''
      }}</span>

    </template>
    <base-column v-for="(columnB, index) in column.columns" :key="`${columnB.key}_${index}`" :column="columnB">
      <!-- 外层(上面父级)没插槽和table组件里面的base-column做映射就看递归里面的base-column(此处children里面的插槽是定义的外面的全部的而不是单个的,找到全部里面对应的插槽映射后内容渲染在children子组件标签然后去递归跟里面slotName渲染的插槽做映射,实际最终组件内部只渲染成一个插槽占位符所以没问题)-->
      <template v-for="(slotItem, slotKey) in $scopedSlots" :slot="slotKey" slot-scope="{ row,$index }">
        <slot v-if="!['optRow'].includes(slotKey)" :name="slotKey" :row="row" :$index="$index" :column="columnB">
        </slot>
      </template>


    </base-column>
  </el-table-column>
</template>

<script>
import RenderDom from './renderDom'

export default {
  name: 'BaseColumn',
  components: { RenderDom },
  props: {
    column: Object,
    tooltipShow:{
      type: Boolean,
      default: true
    }
  },
  data() {
    return {}
  },
  created() { },
  mounted() {

  },
  computed: {},
  watch: {},
  methods: {},
}
</script>
<style lang="less" scoped></style>
