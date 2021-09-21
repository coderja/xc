<template>
  <div class="checkbox">
    <el-form ref="checkform" :model="ruleForm" label-width="100px">
      <el-form-item label="建设：" class="mb-10" prop="build">
        <el-radio-group v-model="ruleForm.build">
          <el-radio
            v-for="(item, i) in buildList"
            :key="i"
            :label="item.label"
          >{{ item.label }}</el-radio>
        </el-radio-group>
      </el-form-item>

      <el-form-item label="" prop="buildChild">
        <el-checkbox-group v-model="ruleForm.buildChild" @change="checkRule">
          <el-checkbox
            v-for="(item, i) in buildChildList"
            :key="i"
            :disabled="!ruleForm.build"
            :label="item.label"
          >{{ item.label }}</el-checkbox>
        </el-checkbox-group>
      </el-form-item>

      <el-form-item label="运维：" prop="operate">
        <el-checkbox-group v-model="ruleForm.operate" @change="checkRule">
          <el-checkbox
            v-for="(item, i) in operationList"
            :key="i"
            :label="item.label"
          >{{ item.label }}</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      {{ ruleForm.type2 }}
      <el-form-item label="服务：" prop="service">
        <el-checkbox-group v-model="ruleForm.service" @change="checkRule">
          <el-checkbox
            v-for="(item, i) in serviceList"
            :key="i"
            :label="item.label"
          >{{ item.label }}</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      ruleForm: {
        build: '',
        buildChild: [],
        operate: [],
        service: []
      },
      buildList: [
        {
          code: '0',
          label: '新建'
        },
        {
          code: '1',
          label: '续建'
        }
      ],
      buildChildList: [
        {
          code: '0',
          label: '软件开发'
        },
        {
          code: '1',
          label: '软件采购'
        },
        {
          code: '2',
          label: '硬件采购'
        }
      ],
      operationList: [
        {
          code: '0',
          label: '软件运维'
        },
        {
          code: '1',
          label: '硬件运维'
        }
      ],
      serviceList: [
        { code: '0', label: '设计咨询' },
        { code: '2', label: '软件测评' },
        { code: '3', label: '租赁服务' },
        { code: '5', label: '监理服务' },
        { code: '6', label: '其他服务' },
        { code: '7', label: '数据服务' },
        { code: '10', label: '架构治理' },
        { code: '9', label: '课题研究' },
        { code: '1', label: '商用密码测评' },
        { code: '8', label: '等级保护评测服务' },
        { code: '11', label: '标准规范制定' }
      ]
    }
  },
  methods: {
    // 将数据传给父页面
    checkRule() {
      this.$emit('proTypeDatas', this.ruleForm)
    }
  }
}
</script>
<style scoped>
::v-deep .el-form-item__label,
::v-deep .el-form-item__content {
  line-height: 16px;
  font-size: 16px;
}
.mb-10 {
  margin-bottom: 10px;
}
</style>
