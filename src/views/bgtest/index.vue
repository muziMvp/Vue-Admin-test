<template>
  <div class="bgtest-container">
    <div class="bg-picture">
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
      <p>编辑一段话描述当前疫情现状</p>
    </div>
    <div class="input-tag">
      <el-tag v-for="tag in dynamicTags" :key="tag" closable :disable-transitions="false" @close="handleClose(tag)">
        {{ tag }}
      </el-tag>
      <el-input 
        class="input-new-tag" 
        v-if="inputVisible" 
        v-model="inputValue" 
        ref="saveTagInput" 
        size="small" 
        @keyup.enter.native="handleInputConfirm" 
        @blur="handleInputConfirm" />
      <!-- </el-input> -->
      <el-button v-else class="button-new-tag" size="small" @click="showInput">+ New Tag</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Bgtest',
  data() {
    return {
      inputValue: '',
      dynamicTags: [],
      inputVisible: false
    }
  },
  methods: {
    handleInputConfirm() {
      let inputValue = this.inputValue
      if (inputValue) {
        this.dynamicTags.push(inputValue)
      }
      this.inputVisible = false
      this.inputValue = ''
      console.log(inputValue, this.inputValue, 'handleInputConfirm')
    },
    showInput() {
      console.log('showInput')
      this.inputVisible = true
      this.$nextTick(() => {
        // this.$refs.saveTagInput.$refs.input.focus()
        console.log(this.$refs.saveTagInput.focus(), 'showInput')
      })
    },
    handleClose(val) {
      this.dynamicTags.splice(this.dynamicTags.indexOf(val), 1)
      console.log(val, 'handleClose')
    }
  }
}
</script>

<style lang="scss" scoped>
// @import "~@/styles/mixin.scss";
.bgtest-container {
  margin:10px 35px ;
  .bg-picture {
    height: 300px;
    overflow: hidden;
    overflow-y: scroll;
    background-image: url('https://img1.baidu.com/it/u=131948171,990039642&fm=253&fmt=auto&app=138&f=JPEG?w=667&h=500');
    // background-image: url('../../assets/img/bg.png');
    background-repeat: no-repeat;
    background-attachment:fixed;
    p {
      color: #fff;
    }
  }
}
.el-tag + .el-tag {
  margin-left: 10px;
}
.button-new-tag {
  margin-left: 10px;
  height: 32px;
  line-height: 30px;
  padding-top: 0;
  padding-bottom: 0;
}
.input-new-tag {
  width: 90px;
  margin-left: 10px;
  vertical-align: bottom;
}
</style>