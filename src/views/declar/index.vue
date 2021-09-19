<template>
  <div class="app-container">
    <div class="steps">
      <el-steps :active="2" align-center>
        <el-step title="项目基本信息">
          <span slot="icon"> <i class="el-icon-edit" /> </span></el-step>
        <el-step title="项目资金信息">
          <span slot="icon"> <i class="el-icon-money" /> </span></el-step>
        <el-step title="项目绩效指标">
          <span slot="icon"> <i class="el-icon-tickets" /> </span></el-step>
      </el-steps>
    </div>

    <project-info />
    <money-info />
    <project-indicator />

    <div class="btns">
      <el-button>返回</el-button>
      <el-button type="primary">暂存</el-button>
      <el-button type="primary" plain>上一步</el-button>
      <el-button type="success">下一步</el-button>
      <el-button type="success">下载申报表</el-button>
      <el-button type="success">提交</el-button>
    </div>
  </div>
</template>

<script>
import moneyInfo from './components/money-info.vue'
import projectInfo from './components/project-info.vue'
import projectIndicator from './components/project-indicator.vue'
export default {
  components: {
    moneyInfo,
    projectInfo,
    projectIndicator
  },
  data() {
    return {
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
    }
  },
  methods: {
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

<style scoped lang='scss'>
.line {
  text-align: center;
}
.steps {
  width: 50%;
  margin: 0 auto;
  margin-top: 28px;
}
.btns {
  text-align: center;
}
::v-deep .el-step.is-horizontal .el-step__line {
    height: 2px;
    top: 19px;
    left: 50%;
    right: -50%;
}
::v-deep .el-step__icon {
  width:40px;
  height:40px;
  font-size: 20px;
}
</style>
