<template>
  <div class="project-tracking">
    <!-- 基本信息 -->
    <div class="implementation-info">
      <div class="implementation-info-item">
        <div class="info-item-title">项目基本信息</div>
        <el-form ref="from" label-width="115px">
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
          <el-form-item label="计划启用年月:">
            <div class="value">2020-09-01</div>
          </el-form-item>
        </el-form>
      </div>
      <div class="implementation-info-item">
        <div class="info-item-title">申报单位基本情况</div>
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
        <div class="info-item-title">实施信息</div>
        <el-form ref="from" label-width="90px">
          <el-form-item label="实施周期:">
            <div class="value">12个月</div>
          </el-form-item>
          <div class="info">
            手段，强化管理能力，实现电子政务项目从申报、立项、招投、实施、变更、验收到绩效评估的全流程线上管理。
          </div>
        </el-form>
      </div>
    </div>
    <div class="implementation-table-details">
      <div class="table-title">项目实施进度报告上传</div>
      <div class="bidding-announcement" v-show="!isAll"> 
        <p>注：请上传项目实施进度报告和架构治理要求自查报告</p>
        <el-button type="primary">请上传项目实施进度报告模板</el-button>
      </div>
      <div class="table-details">
        <el-table
          :data="tableData"
          border
          style="width: 100%">
          <el-table-column
            type="index"
            label="序号"
            width="100"
            align="center"
          />
          <el-table-column
            prop="trackingData"
            label="跟踪资料"
            width="420"
            align="center"
          />
          <el-table-column
            prop="reportType"
            label="报告类型"
            width="210"
            align="center"
          />
          <el-table-column
            prop="reportingCycle"
            label="报告周期"
            width="220"
            align="center"
          />
          <el-table-column
            prop="selfEvaluator"
            label="自评人"
            width="220"
            align="center"
          />
          <el-table-column
            prop="selfAssessmentTime"
            label="自评时间"
            width="220"
            align="center"
          />
          <el-table-column
            fixed="right"
            label="操作"
            align="center"
            width="250">
            <template slot-scope="scope">
              <el-button v-show="!scope.row.fileUrl && !isAll" @click="handleClick(scope.row)" type="primary" size="small">上传</el-button>
              <el-button v-show="scope.row.fileUrl" type="primary" size="small">下载</el-button>
              <el-button v-show="scope.row.fileUrl" type="success" size="small">预览</el-button>
              <el-button v-show="scope.row.fileUrl" type="danger" size="small">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <div class="submit-btn">
      <el-button type="info">返回</el-button>
      <el-button v-show="!isAll" type="primary" @click="submit">提交</el-button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tableData: [{
        trackingData: '招标文件',
        reportType: '月报',
        reportingCycle: '2021-09-21',
        selfEvaluator: '',
        selfAssessmentTime: ''
      }],
      biddingAnnouncement: '', // 招标公告链接
      isAll: '',
    }
  },
  created() {
    this.isAll = this.$route.query.type === 'all'
  },
  methods: {
    /**
     * 点击提交的事件
     */
    submit() {
      if (!this.biddingAnnouncement && !this.tableData[0].fileUrl) {
        this.$message.error('请填写招标公告链接或者上传招标文件！')
        return
      }
    }
  }
}
</script>
<style scoped lang="scss">
.project-tracking {
  width: 100%;
  height: 100%;
  padding: 15px 15px;
  p {
    margin: 0;
    padding: 0;
  }
  ::v-deep label {
    font-weight: 500;
    color: #666;
  }
  .implementation-info {
    display: flex;
    .implementation-info-item {
      width: calc(100% / 3);
      margin: 15px;
      box-shadow: 0 0 10px #E6E6E6;
      .info-item-title {
        padding: 15px 20px;
        background: #eef4ff;
        color:#4180e8;
        font-size: 20px;
      }
      ::v-deep .el-form {
        padding: 0 20px 20px 20px;
      }
      ::v-deep .el-form-item__label {
        text-align: left;
        font-size: 16px;
        font-weight: bold;
      }
      .info {
        color: #666;
        font-size: 14px;
        line-height: 20px;
      }
      ::v-deep .el-form-item {
        margin-bottom: 0;
      }
    }
  }
  .implementation-table-details {
    width: 100%;
    padding: 0 10px;
    .table-title {
      color: #0d2eb7;
      font-size: 20px;
      padding: 15px 0;
      font-weight: bold;
    }
    .bidding-announcement {
      padding: 10px 0;
      p {
        font-size: 16px;
        font-weight: bold;
        color: #01AAED;
        padding-bottom: 15px;
      }
      ::v-deep .el-input {
        width: 30%;
      }
    }
  }
  .submit-btn {
    width: 100%;
    display: flex;
    padding: 35px 0 20px 0;
    justify-content: center;
    ::v-deep .el-button {
      padding: 12px 33px;
    }
  }
}
</style>
