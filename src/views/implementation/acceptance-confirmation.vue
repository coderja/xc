<template>
  <div class="acceptance-record">
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
      <div class="table-title">项目验收自查报告和项目验收准备材料</div>
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
            prop="documentClassification"
            label="文件分类"
            width="380"
            align="center"
          />
          <el-table-column
            prop="fileName"
            label="文件名称"
            width="420"
            align="center"
          />
          <el-table-column
            prop="uploader"
            label="上传人"
            width="360"
            align="center"
          />
          <el-table-column
            prop="updateTime"
            label="上传时间"
            width="250"
            align="center"
          />
          <el-table-column
            fixed="right"
            label="操作"
            align="center"
            width="250">
            <template slot-scope="scope">
              <el-button v-show="scope.row.fileUrl" type="primary" size="small">下载</el-button>
              <el-button v-show="scope.row.fileUrl" type="success" size="small">预览</el-button>
            </template>
          </el-table-column>
        </el-table>
        <div class="select-test-run">
          <div class="text-run-left">
            <p><i class="red">*</i>是否试运行：</p>
            <el-radio :disabled="true" v-model="isTestRun" label="1">是</el-radio>
            <el-radio :disabled="true" v-model="isTestRun" label="2">否</el-radio>
          </div>
          <div class="text-run-left" v-show="isTestRun === '1'">
            <p><i class="red">*</i>试运行时间：</p>
            <el-date-picker
              v-model="testRunDate"
              type="date"
              placeholder="选择日期"
              :disabled="true"
            />
          </div>
        </div>
      </div>
    </div>
    <div class="implementation-table-details">
      <div class="table-title">审核意见</div>
      <div class="bidding-announcement">
        <p><i class="red">*</i>变更依据：</p><el-input :rows="3" type="textarea" v-model="reviewComments" placeholder="请输入审核意见" />
      </div>
    </div>
    <div class="submit-btn">
      <el-button type="info">返回</el-button>
      <el-button type="danger">退回</el-button>
      <el-button type="primary" @click="submit">通过</el-button>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tableData: [{
        documentClassification: '验收自查报告',
        uploader: 'lll',
        fileName: '验收自查报告',
        updateTime: '2021-02-21',
        fileUrl: ''
      }, {
        uploader: 'lll',
        documentClassification: '验收自查报告',
        fileName: '项目验收准备材料',
        updateTime: '2021-02-21',
        fileUrl: 'xxxx'
      }],
      reviewComments: '', // 审核意见
      isTestRun: '', // 是否试运行
    }
  },
  methods: {
    /**
     * 点击提交的事件
     */
    submit() {
      if (!this.reviewComments) {
        this.$message.error('请填写审核意见')
        return
      }
    },
    // 审核成功的方法
    reviewSuccess() {
      this.$message.success('提交成功')
      setTimeout(() => {
        this.$route.push({
          path: '/all-implementation'
        })
      }, 1000)
    },
    /**
     * 审核失败的方法
     */
    reviewError() {
      this.$message.error('提交失败，请重新提交')
    }
  }
}
</script>
<style scoped lang="scss">
.acceptance-record {
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
    .red {
          color: red;
          font-size: 16px;
          margin-right: 2px;
        }
    .table-title {
      color: #0d2eb7;
      font-size: 20px;
      padding: 15px 0;
      font-weight: bold;
    }
    .select-test-run {
      width: 100%;
      display: flex;
      margin-top: 20px;
      justify-content: space-between;
      .text-run-left {
        display: flex;
        align-items: center;
        p {
          font-size: 16px;
          color: #666;
        }
      }
    }
    .bidding-announcement {
      display: flex;
      padding: 15px 0;
      p {
        font-size: 16px;
        font-weight: bold;
        color: #666;
      }
      ::v-deep .el-input, .el-textarea {
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

