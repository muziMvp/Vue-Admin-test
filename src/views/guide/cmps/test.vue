<template>
  <el-dialog :visible="dialogFormVisible">
    <el-form :model="form1">
      <el-form-item label="活动名称" :label-width="formLabelWidth">
        <el-input v-model="form1.name" autocomplete="off"></el-input>
      </el-form-item>
    <!-- <el-form-item label="活动区域" :label-width="formLabelWidth">
      <el-select v-model="form.region" placeholder="请选择活动区域">
        <el-option label="区域一" value="shanghai"></el-option>
        <el-option label="区域二" value="beijing"></el-option>
      </el-select>
    </el-form-item> -->
    <el-form-item :label-width="formLabelWidth">
      <el-button type="default" @click="addBtn()">新增</el-button>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
  </div>
  </el-dialog>
</template>
<script>
export default {
  name: 'Test',
  props: {
    form: {
      type: Object,
      default: () => {}
    },
    hasAdd: {
      type: Function,
      default: null
    }
  },
  data() {
    return {
      dialogFormVisible: true,
      formLabelWidth: '100px',
      form1: {
        name: ''
      }
    }
  },
  created() {
    // this.form1 = this.form
    // this.form1 = { ...this.form }
    Object.assign(this.form1, this.form)
    console.log(this.form1.name, 'created--form1')
  },
  mounted() {
    console.log(this.form1.name, 'mounted--form1')
  },
  methods: {
    addBtn() {
      if (this.hasAdd) {
        this.hasAdd()
        console.log('ccccc')
      }
    }
  }
}

</script>