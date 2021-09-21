<template>
  <div class="app-container">
    <div class="steps">
      <el-steps :active="active" align-center>
        <el-step title="项目基本信息">
          <span slot="icon"> <i class="el-icon-edit" /> </span></el-step>
        <el-step title="项目资金信息">
          <span slot="icon"> <i class="el-icon-money" /> </span></el-step>
        <el-step title="项目绩效指标">
          <span slot="icon"> <i class="el-icon-tickets" /> </span></el-step>
      </el-steps>
    </div>

    <project-info v-show="active == 1" />
    <money-info v-show="active == 2" />
    <project-indicator v-show="active == 3" />

    <div class="btns">
      <el-button @click="goBack">返回</el-button>
      <el-button type="primary">暂存</el-button>
      <el-button
        v-if="active > 1"
        type="primary"
        plain
        @click="prev"
      >上一步</el-button>
      <el-button type="success" @click="next" v-if='active < 3'>下一步</el-button>
      <el-button type="success" v-if='active === 3'>下载申报表</el-button>
      <el-button type="success" v-if='active === 3'>提交</el-button>
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
      active: 1,
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
    next() {
      if (this.active < 3 && this.active > 0) {
        this.active++
      }
    },
    prev() {
      if (this.active > 1) {
        this.active--
      }
    },
    goBack() {
      this.$router.push({ path: 'mydeclar' })
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
.line {
  text-align: center;
}
.steps {
  width: 50%;
  margin: 0 auto;
  margin-top: 28px;
}
.btns {
  position: absolute;
    left: 50%;
    transform: translate(-50%);
    bottom: 20px;
}
::v-deep .el-step.is-horizontal .el-step__line {
  height: 2px;
  top: 19px;
  left: 50%;
  right: -50%;
}
::v-deep .el-step__icon {
  width: 40px;
  height: 40px;
  font-size: 20px;
}
</style>
