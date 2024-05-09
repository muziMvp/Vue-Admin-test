<template>
  <div class="contianer">
    <div class="operate">
      <span>我是子组件：</span>
      <el-button @click="count()">传值给父组件</el-button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Child',
  props: {
    fatherNum: {
      type: Number,
      default: null
    }
  },
  data() {
    return {
      num: 0
    }
  },
  created() {
    this.num = this.fatherNum
    console.log(this.num, 'child-created', this.father)
  },
  mounted() {
    // this.$nextTick(() => { 
    //   this.num = this.fatherNum
    console.log(this.num, 'child-mounted', this.father)
    // })
  },
  methods: {
    count() {
      console.log(this.num, 'child-count')
      this.num++ 
      this.$emit('getValue', this.num)
    }
  }
}
</script>