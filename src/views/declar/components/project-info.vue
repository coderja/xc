<template>
  <div class="app-container">
    <el-form
      ref="pojectInfoForm"
      :model="form"
      :rules="formRules"
      :disabled='isDetail'
      label-width="120px"
    >
      <form-label-title>
        <span slot="title">
          项目基本信息
        </span>
      </form-label-title>
      <el-row>
        <el-col :span="8">
          <el-form-item label="项目编号" prop="proNum">
            <el-input v-model="form.proNum" disabled />
          </el-form-item>
        </el-col>
        <el-col :span="8" class="col-text-tip">
          <span>注：项目编号默认为集中申报的项目编号</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="8" class="txt-overflow">
          <el-form-item label="项目类型" prop="proType">
            <el-tooltip
              class="item"
              effect="dark"
              :content="form.proType"
              :disabled="!form.proType"
              placement="top-start"
            >
              <el-input
                ref='protype'
                v-model="form.proType"
                @focus="proTypeDialogVisible = true"
              />
            </el-tooltip>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="项目名称" prop="proName">
            <el-input v-model="form.proName" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="项目性质" prop="protNature">
            <el-select v-model="form.protNature" placeholder="请选择项目性质">
              <el-option
                v-for="(item, i) in projectNatureList"
                :key="i"
                :value="item.code"
                :label="item.label"
              />
            </el-select>
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="申报类型" prop="declarType">
            <el-radio-group v-model="form.declarType">
              <el-radio label="年度申报项目" />
              <el-radio label="追加申报项目" />
            </el-radio-group>
          </el-form-item>
        </el-col>

        <el-col v-if="isShowLastSysName" :span="8">
          <el-form-item label="往期系统名称" prop="lastSysName">
            <el-input v-model="form.lastSysName" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="计划启用年月" prop="planStartDate">
            <el-date-picker
              v-model="form.planStartDate"
              type="date"
              value-format="timestamp"
              placeholder="选择日期"
            />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="16" class="col-text-tip">
          <el-form-item label="">
            <span>注：每年第三季度申报下一年度项目的，选择申报类型为“集中申报项目”，除此之外选择“追加项目”。</span>
          </el-form-item>
        </el-col>
      </el-row>

      <form-label-title>
        <span slot="title">
          申报单位基本情况
        </span>
      </form-label-title>
      <el-row>
        <el-col :span="8">
          <el-form-item label="申报单位" prop="declarComp">
            <el-input v-model="form.declarComp" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="实施单位" prop="explEntity">
            <el-input v-model="form.explEntity" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="项目负责人" prop="produtyOfficer">
            <el-input v-model="form.produtyOfficer" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="负责人手机" prop="DutyPhoneNum">
            <el-input v-model="form.DutyPhoneNum" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="项目联系人" prop="proContacts">
            <el-input v-model="form.proContacts" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="负责人手机" prop="dutyOfficerPhone">
            <el-input v-model="form.dutyOfficerPhone" />
          </el-form-item>
        </el-col>
      </el-row>

      <form-label-title>
        <span slot="title">
          实施信息
        </span>
      </form-label-title>

      <el-row>
        <el-col :span="8">
          <el-form-item label="项目周期" prop="projectCycle">
            <el-input v-model="form.projectCycle" />
          </el-form-item>
        </el-col>
        <el-col :span="12" class="col-text-tip text-suffix">
          <span>个月</span>
        </el-col>
      </el-row>

      <el-row>
        <el-col :span="24">
          <el-form-item label="实施内容" prop="implemContent">
            <el-input v-model="form.implemContent" type="textarea" />
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>

    <el-dialog
      class="dialog"
      top="15%"
      title="选择项目类型"
      :append-to-body="true"
      width="600px"
      :visible.sync="proTypeDialogVisible"
      @open="openDialog"
    >
      <dataTypeCheckbox ref="proTypeForm" @proTypeDatas="proTypeDatas" />
      <div style="text-align:center">
        <el-button type="primary" @click="checkRule">确定</el-button>
        <el-button type="success" @click="resetForm">重置</el-button>
      </div>
    </el-dialog>

    <el-dialog
      class="dialog"
      top="5%"
      :append-to-body="true"
      width="50%"
      title="政务需求总量"
      :visible.sync="isShowReqTotal"
      @open="openDialog"
    >
      <req-total-form />
      <div class="contract-btn" style="text-align:center">
        <el-button type="info" @click="checkRule">返回</el-button>
        <el-button type="primary" @click="resetForm">保存</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import dataTypeCheckbox from '@/views/declar/components/dataType/checkbox.vue'
import reqTotalForm from '@/views/declar/components/reqTotal/form.vue'
export default {
  components: {
    dataTypeCheckbox,
    reqTotalForm
  },
  data() {
    return {
      projectNatureList: [
        { label: '基础支撑', code: '0' },
        { label: '优政兴业惠民', code: '1' },
        { label: '行政审批', code: '2' },
        { label: '监督管理', code: '3' },
        { label: '社会综治', code: '4' },
        { label: '业务能力提升', code: '5' }
      ],
      isShowReqTotal: false,
      proTypeDialogVisible: false,
      protDataForm: {},
      form: {
        proNum: '', // 项目编号
        proType: '', // 项目类型
        proName: '', // 项目名称
        protNature: '', // 项目性质
        declarType: '', // 申报类型
        lastSysName: '', // 往期系统名称
        planStartDate: '', // 计划启用年月
        declarComp: '', // 申报单位
        explEntity: '', // 实施单位
        produtyOfficer: '', // 项目负责人
        DutyPhoneNum: '', // 负责人手机
        proContacts: '', // 项目联系人
        dutyOfficerPhone: '', // 联系人手机
        projectCycle: '', // 项目周期
        implemContent: '' // 实施内容
      },
      formRules: {
        proNum: [
          {
            required: true,
            message: '项目编码必填',
            trigger: 'blur'
          }
        ], // 项目编号
        proType: [
          {
            required: true,
            message: '项目类型必填'
            // trigger: 'blur'
          }
        ], // 项目类型
        proName: [
          {
            required: true,
            message: '项目类型必填',
            trigger: 'change'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ], // 项目名称
        protNature: [
          {
            required: true,
            message: '项目性质必填',
            trigger: 'change'
          }
        ], // 项目性质
        declarType: [
          {
            required: true,
            message: '项目性质必填',
            trigger: 'change'
          }
        ], // 申报类型
        lastSysName: [
          {
            required: true,
            message: '往期系统名称必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ], // 往期系统名称
        planStartDate: [
          {
            required: true,
            message: '计划启用年月必填',
            trigger: 'change'
          }
        ], // 计划启用年月
        declarComp: [
          {
            required: true,
            message: '申报单位必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ], // 申报单位
        explEntity: [
          {
            required: true,
            message: '实施单位必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ], // 实施单位
        produtyOfficer: [
          {
            required: true,
            message: '项目负责人必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ], // 项目负责人
        DutyPhoneNum: [
          {
            required: true,
            message: '负责人手机必填',
            trigger: 'blur'
          },
          {
            pattern: /^[1](([3][0-9])|([4][5-9])|([5][0-3,5-9])|([6][5,6])|([7][0-8])|([8][0-9])|([9][1,8,9]))[0-9]{8}$/,
            message: '请输入正确的手机号格式',
            trigger: 'blur'
          }
        ], // 负责人手机
        proContacts: [
          { required: true, message: '项目联系人必填', trigger: 'blur' },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ], // 项目联系人
        dutyOfficerPhone: [
          {
            required: true,
            message: '联系人手机必填',
            trigger: 'blur'
          },
          {
            pattern: /^[1](([3][0-9])|([4][5-9])|([5][0-3,5-9])|([6][5,6])|([7][0-8])|([8][0-9])|([9][1,8,9]))[0-9]{8}$/,
            message: '请输入正确的手机号格式',
            trigger: 'blur'
          }
        ], // 项目联系人手机
        projectCycle: [
          {
            required: true,
            message: '项目周期必填',
            trigger: 'blur'
          },
          {
            pattern: /^[0-9]{0,}(\.[0-9]{0,})?$/,
            message: '只输入数字',
            trigger: 'blur'
          }
        ], // 项目周期
        implemContent: [
          {
            required: true,
            message: '实施内容必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ] // 实施内容
      }
    }
  },
  computed: {
    isShowLastSysName() {
      const proType = this.form.proType
      return (
        proType.includes('软件运维') ||
        proType.includes('软件开发') ||
        proType.includes('软件采购')
      )
    },
    isDetail() {
      return this.$route.query.detail
    }
  },
  methods: {
    onSubmit() {
      this.$message('submit!')
    },
    // 重置数据类型表单
    resetForm() {
      this.$refs.proTypeForm.$refs.checkform.resetFields()
    },
    // 数据类型字符串组装
    checkRule() {
      const build = this.protDataForm.build
      const buildChild = this.protDataForm.buildChild
      const service = this.protDataForm.service
      const operate = this.protDataForm.operate
      if (buildChild.length && operate.length) {
        this.$message({
          message: '建设类和运维类不能同时选择',
          type: 'error'
        })
        return
      }
      if (service.length && operate.length) {
        this.$message({
          message: '运维类和服务类不能同时选择',
          type: 'error'
        })
        return
      }
      if (
        build &&
        buildChild.length == 0 &&
        (service.length || operate.length)
      ) {
        this.$message({
          message: '建设类信息不完整',
          type: 'error'
        })
        return
      }

      let buildStr = []
      let operateStr = []
      let serviceStr = []
      this.form.proType = []
      if (buildChild.length && build) {
        buildChild.map(el => {
          buildStr.push(`建设/${build}/${el}`)
        })
        buildStr = buildStr.join(',')
        this.form.proType.push(buildStr)
      }

      if (operate.length) {
        operate.map(el => {
          operateStr.push(`运维/${el}`)
        })
        operateStr = operateStr.join(',')
        this.form.proType.push(operateStr)
      }

      if (service.length) {
        service.map(el => {
          serviceStr.push(`服务/${el}`)
        })
        serviceStr = serviceStr.join(',')
        this.form.proType.push(serviceStr)
      }
      this.form.proType = this.form.proType.join(',')
      this.$emit('handle-project-type', this.form.proType)
      this.proTypeDialogVisible = false
    },
    proTypeDatas(data) {
      this.protDataForm = data
    },
    openDialog() {
      this.$nextTick(() => {})
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
::v-deep .txt-overflow .el-input__inner {
  overflow: hidden;
  text-overflow: ellipsis;
}
::v-deep .el-form{margin-bottom: 30px;}
</style>
