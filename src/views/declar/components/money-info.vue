<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="140px">
      <div class="table-list">
        <h3>项目估算资金（万元）</h3>

        <div class="table">
          <form-label-title class="list">
            <span slot="title">
              资金来源（万元）
            </span>
          </form-label-title>
          <div class="t-operate-btn">
            <span class="text-info">合计</span>
            <span class="text-info">0</span>
            <el-button type="primary" size="small">模板下载</el-button>
            <el-button type="primary" size="small">数据导入</el-button>
            <el-button type="danger" size="small">删除</el-button>
          </div>
        </div>
      </div>

      <el-row>
        <el-col :span="8">
          <el-form-item label="市级财政估算类安排">
            <el-input v-model="form.cityPlan" />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="8">
          <el-form-item label="其他来源">
            <el-input v-model="form.otherOriginal" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="其他来源说明">
            <el-input v-model="form.originalExplain" />
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
import table from './table/table.vue'
export default {
  data() {
    return {
      form: {
        cityPlan: '',
        otherOriginal: '',
        cityPoriginalExplainlan: ''
      }
    }
  },
  componnets: {
    table
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

<style scoped lang="scss">
@import '../assets/index.scss';
.table-list {
  border: 1px solid #e7e7e7;
  margin-bottom: 12px;
  h3 {
    position: relative;
    background: #fff;
    top: -30px;
    left: 12px;
    width: fit-content;
  }
  .t-operate-btn {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    margin-bottom: 12px;
    .text-info {
      margin-right: 12px;
    }
  }
  .table {
    border-top: 1px solid #e7e7e7;
    width: 98%;
    margin: 0 auto;
    .list {
      position: relative;
      background: #fff;
      top: -12px;
      left: 12px;
      width: fit-content;
    }
  }
}
</style>
