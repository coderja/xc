<template>
  <div class="app-container">
      <div class="table-list">
        <h3>项目估算资金（万元）</h3>

        <s-d-table
          ref='SDForm'
          v-if="!proType.includes('软件开发')"
        />

        <s-p-table
        ref='SPForm'
          v-if="!proType.includes('软件采购')"
        />

        <h-p-table ref='HPForm' v-if="!proType.includes('硬件采购')" />

        <service-table ref='SForm'  v-if="!proType.includes('服务')" />

        <s-o-table ref='SOForm' v-if="!proType.includes('软件运维')" />

        <h-o-table  ref='HOForm' v-if="!proType.includes('硬件运维')" />
      </div>

    <el-form ref="moneyInfoForm" :model="form" :disabled='isDetail' label-width="160px">
      <form-label-title class="list">
        <span slot="title">
          资金来源（万元）
        </span>
      </form-label-title>
      <el-row>
        <el-col :span="8">
          <el-form-item
            label="市级财政估算类安排"
            prop="cityPlan"
            :rules="formRules.cityPlan"
          >
            <el-input v-model="form.cityPlan" />
          </el-form-item>
        </el-col>
      </el-row>
      <el-row class="mt-10">
        <el-col :span="8">
          <el-form-item
            label="其他来源"
            prop="otherOriginal"
            :rules="formRules.otherOriginal"
          >
            <el-input v-model="form.otherOriginal" />
          </el-form-item>
        </el-col>
        <el-col :span="8">
          <el-form-item
            label="其他来源说明"
            prop="originalExplain"
            :rules="formRules.originalExplain"
          >
            <el-input v-model="form.originalExplain" />
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
import hardPurchasTable from './capitalTable/hardPurchasTable.vue'
import hardOperateTable from './capitalTable/hardOperateTable.vue'
import serviceTable from './capitalTable/serviceTable.vue'
import softDevelopTable from './capitalTable/softDevelopTable.vue'
import softOperateTable from './capitalTable/softOperateTable.vue'
import softPurchasTable from './capitalTable/softPurchasTable.vue'
export default {
  components: {
    'h-p-table': hardPurchasTable,
    'h-o-table': hardOperateTable,
    'service-table': serviceTable,
    's-d-table': softDevelopTable,
    's-o-table': softOperateTable,
    's-p-table': softPurchasTable
  },
  props: {
    proType: String
  },
  computed: {
    isDetail() {
      return this.$route.query.detail
    }
  },
  data() {
    return {
      total: '',
      form: {
        cityPlan: '',
        otherOriginal: '',
        cityPoriginalExplainlan: ''
      },
      formRules: {
        cityPlan: [
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '请输入中英文数字',
            trigger: 'blur'
          },
          {
            required: true,
            message: '市级财政估算类安排必填',
            trigger: 'blur'
          }
        ],
        otherOriginal: [
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '请输入中英文数字',
            trigger: 'blur'
          },
          {
            required: true,
            message: '其他来源必填',
            trigger: 'blur'
          }
        ],
        originalExplain: [
          {
            required: false,
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '请输入中英文数字',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    calcTotalVal(v) {
      console.log(v)
      this.total = v
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
    margin-bottom: 50px;
    .list {
      position: relative;
      background: #fff;
      top: -12px;
      left: 12px;
      width: fit-content;
    }
  }
  .table:last-child {
    margin-bottom: 2px;
  }
}
.mt-10 {
  margin-bottom: 50px;
}
</style>
