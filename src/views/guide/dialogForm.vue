<template>
  <el-dialog :visible="isShow" width="60%" :before-close="handleClose">
    <div class="content">
      <div class="main">
        <el-form ref="formRef" :model="sizeForm" :rules="ruleForm" :inline="true" label-width="120px" size="mini">
          <el-form-item label="工程项目名称" prop="itemName">
            <el-input v-model="sizeForm.itemName"></el-input>
          </el-form-item>
          <el-form-item label="参建单位">
            <el-select v-model="sizeForm.companyName" placeholder="请选择活动区域">
              <el-option label="区域一" value="shanghai"></el-option>
              <el-option label="区域二" value="beijing"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="送审金额">
            <el-input v-model="sizeForm.giveMoney"></el-input>
          </el-form-item>
          <el-form-item label="初审金额">
            <el-input v-model="sizeForm.firstMoney"></el-input>
          </el-form-item>
          <el-form-item label="审定金额">
            <el-input v-model="sizeForm.auditMoney"></el-input>
          </el-form-item>
        </el-form>
      </div>
      <div class="footer">
        <el-button type="default" @click="handleClose">取消</el-button>
        <el-button type="primary" @click="submit">确定</el-button>
      </div>
    </div>
  </el-dialog>
</template>
<script>
export default {
  name: 'DialogForm',
  props: {
    isFlag: {
      type: Number,
      default: null
    },
    num: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      isShow: false,
      sizeForm: {
        itemName: '',
        companyName: '',
        giveMoney: '',
        firstMoney: '',
        auditMoney: ''
      },
      ruleForm: {
        itemName: [
          { required: true, message: '请输入活动名称', trigger: 'blur' }
          // { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ]
      },
      list: [
        {
          name: 'haier',
          age: 45
        },
        {
          name: 'geli',
          age: 35
        }
      ]
    }
  },
  created() {
    console.log(this.isShow, 'created')
    console.log(this.isFlag, 'created-isFlag')
  },
  mounted() {
    console.log(this.isShow, 'mounted')
    console.log(this.isFlag, 'mounted-isFlag')
  },
  methods: {
    // 初始化
    init(val) {
      this.sizeForm.itemName = val.itemName
      this.sizeForm.companyName = val.companyName
      this.sizeForm.giveMoney = val.giveMoney
      this.sizeForm.firstMoney = val.firstMoney
      this.sizeForm.auditMoney = val.auditMoney
      this.isShow = true
    },
    submit() {
      // this.$refs['formRef'].validate((valid) => {
      //   if (valid) {
      //     alert('submit!');
      //   } else {
      //     console.log('error submit!!');
      //     return false;
      //   }
      // });
      this.$emit('refreshData', this.sizeForm.itemName)
      this.handleClose()
    },
    handleClose() {
      this.$emit('update:num', 'helloooo')
      this.isShow = false
      this.sizeForm = {}
    }
  }
}
</script>