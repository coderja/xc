<template>
  <div class="app-container">
    <el-form ref="projectIndicForm" :disabled='isDetail' :model="form" label-width="120px">
      <form-label-title>
        <span slot="title">
          项目绩效指标
        </span>
      </form-label-title>
      <el-row v-for="(item, i) in form.indicatorFormList" :key="i">
        <el-col :span="9">
          <el-form-item
            label="绩效指标"
            :rules="modelDataRules.indicator"
            :prop="'indicatorFormList.' + i + '.indicator'"
          >
            <el-input v-model="item.indicator" />
          </el-form-item>
        </el-col>
        <el-col :span="9">
          <el-form-item
            label="目标值"
            :rules="modelDataRules.targetValue"
            :prop="'indicatorFormList.' + i + '.targetValue'"
          >
            <el-input v-model="item.targetValue" />
          </el-form-item>
        </el-col>

        <el-col :span="3" class="text-center mt-5">
          <el-button
            v-if="i == 0 && !isDetail"
            type="primary"
            size="small"
            @click="addItem"
          >新增</el-button>
          <el-button
            v-if="i != 0 && !isDetail"
            type="danger"
            size="small"
            @click="deleteItem(i)"
          >删除</el-button>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    // let validateNum = (rule, value, callback) => {
    //   let regStr = /^(\d+)(\.\d+)?$/;
    //   if (value && !regStr.test(value)) {
    //     callback(new Error('请输入数字'));
    //   } else {
    //     callback();
    //   }
    // };
    return {
      form: {
        indicatorFormList: [
          { indicator: '', targetValue: '' },
          { indicator: '', targetValue: '' },
          { indicator: '', targetValue: '' }
        ]
      },
      modelDataRules: {
        indicator: [
          {
            required: true,
            message: '绩效指标必填',
            trigger: 'blur'
          },
          {
            pattern: /^[0-9]{0,}(\.[0-9]{0,})?$/,
            message: '只输入数字',
            trigger: 'blur'
          }
        ],
        targetValue: [
          {
            required: true,
            message: '目标值必填',
            trigger: 'blur'
          },
          {
            pattern: /^[0-9]{0,}(\.[0-9]{0,})?$/,
            message: '只输入数字',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  computed: {
    isDetail() {
      return this.$route.query.detail
    }
  },
  methods: {
    addItem() {
      this.form.indicatorFormList.push({ indicator: '', targetValue: '' })
    },
    deleteItem(i) {
      this.form.indicatorFormList.splice(i, 1)
    },
    onSubmit() {
      this.$message('submit!')
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped lang="scss">
@import '../assets/index.scss';
::v-deep .el-form {
  margin-bottom: 30px;
}
</style>
