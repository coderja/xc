<template>
  <div class="implementation-filing">
    <div class="implementation-info">
      <div class="implementation-info-item">
        <div class="title">项目基本信息</div>
        <el-form ref="from" label-width="105px">
           <el-form-item label="项目编号:">
            <div class="value">2020-09-12</div>
          </el-form-item>
          <el-form-item label="项目类型:">
            <div class="value">建设/新建/软件开发</div>
          </el-form-item>
          <el-form-item label="项目名称:">
            <div class="value">项目全生命周期管理平台（一期）</div>
          </el-form-item>
          <el-row>
            <el-col :span='12'>
              <el-form-item label="项目性质:">
                <div class="value">基础支撑</div>
              </el-form-item>
            </el-col>
            <el-col :span='12'>
              <el-form-item label="申报类型:">
                <div class="value">年度申报项目</div>
              </el-form-item>
            </el-col>
          </el-row>
          <el-form-item label="项目性质:">
            <div class="value">基础支撑</div>
          </el-form-item>
          <el-form-item label="计划启用年月:">
            <div class="value">2020-09-01</div>
          </el-form-item>
        </el-form>
      </div>
      <div class="implementation-info-item">
        <div class="title">申报单位基本情况</div>
        <el-form ref="from" label-width="100px">
          <el-form-item label="申报单位:">
            <div class="value">信息中西</div>
          </el-form-item>
          <el-form-item label="实施单位:">
            <div class="value">信息中西</div>
          </el-form-item>
          <el-row>
            <el-col :span='12'>
              <el-form-item label="项目负责人:">
                <div class="value">张强</div>
              </el-form-item>
            </el-col>
            <el-col :span='12'>
              <el-form-item label="负责人手机:">
                <div class="value">18812341234</div>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span='12'>
              <el-form-item label="项目联系人:">
                <div class="value">张强</div>
              </el-form-item>
            </el-col>
            <el-col :span='12'>
              <el-form-item label="联系人手机:">
                <div class="value">18812341234</div>
              </el-form-item>
            </el-col>
          </el-row>
        </el-form>
      </div>
      <div class="implementation-info-item">
        <div class="title">实施信息</div>
        <el-form ref="from" label-width="80px">
          <el-form-item label="项目编号:">
            <div class="value">12个月</div>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {

    }
  }
}
</script>
<style scoped lang="scss">
.implementation-filing {
  width: 100%;
  height: 100%;
  padding: 15px 15px;
  .implementation-info {
    display: flex;
    .implementation-info-item {
      width: calc(100% / 3);
      margin: 15px;
      padding: 20px 20px 20px 15px;
      box-shadow: 0 0 10px #999;
      ::v-deep .el-form-item {
        margin-bottom: 0;
      }
    }
  }
}
</style>
