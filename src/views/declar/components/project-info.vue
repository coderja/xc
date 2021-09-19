<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="140px">
      <form-label-title>
        <span slot="title">
          项目基本信息
        </span>
      </form-label-title>
      <el-row>
        <el-col :span="8">
          <el-form-item label="项目编号">
            <el-input v-model="form.proNum" disabled />
          </el-form-item>
        </el-col>
        <el-col :span="8" class="col-text-tip">
          <span>注：项目编号默认为集中申报的项目编号</span>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="8">
          <el-form-item label="项目类型">
            <el-input v-model="form.proType" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="项目名称">
            <el-input v-model="form.proName" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="项目性质">
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
          <el-form-item label="申报类型">
            <el-radio-group v-model="form.declarType">
              <el-radio label="年度申报项目" />
              <el-radio label="追加申报项目" />
            </el-radio-group>
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="往期系统名称">
            <el-input v-model="form.lastSysName" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="计划启用年月">
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
          <el-form-item label="申报单位">
            <el-input v-model="form.declarComp" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="实施单位">
            <el-input v-model="form.explEntity" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="项目负责人">
            <el-input v-model="form.produtyOfficer" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="负责人手机">
            <el-input v-model="form.DutyPhoneNum" />
          </el-form-item>
        </el-col>

        <el-col :span="8">
          <el-form-item label="项目联系人">
            <el-input v-model="form.proContacts" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item label="负责人手机">
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
          <el-form-item label="项目周期">
            <el-input v-model="form.projectCycle" />
          </el-form-item>
        </el-col>
        <el-col :span="12" class="col-text-tip text-suffix">
          <span>个月</span>
        </el-col>
      </el-row>

      <el-row>
        <el-col :span="24">
          <el-form-item label="实施内容">
            <el-input v-model="form.implemContent" type="textarea" />
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
import formLabelTitle from '@/views/common-components/form-label-title.vue'
export default {
  components: { formLabelTitle },
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
        proContacts: '', // 联系人手机
        dutyOfficerPhone: '', // 项目编号
        projectCycle: '', // 项目周期
        implemContent: '' // 实施内容
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

<style scoped lang="scss">
@import '../assets/index.scss';
</style>
