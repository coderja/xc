<template>
  <div class="add-contract table">
    <form-label-title class="list">
      <span slot="title">
        合同说明
      </span>
    </form-label-title>
    <div class="add-contract-from">
      <el-form ref="form" :model="form" label-width="120px" :rules="formRules">
        <el-row>
          <el-col :span="10">
            <el-form-item label="合同名称" prop="contractName">
              <el-input v-model="form.contractName" :disabled="isAll" />
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="合同金额" prop="contractAmount">
              <el-input v-model="form.contractAmount" :disabled="isAll" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <el-form-item label="甲方名称" prop="partyAName">
              <el-input v-model="form.partyAName" :disabled="isAll" />
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="乙方名称" prop="partyBName">
              <el-input v-model="form.partyBName" :disabled="isAll" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <el-form-item label="甲方代码" prop="partyACode">
              <el-input v-model="form.partyACode" :disabled="isAll" />
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="乙方代码" prop="partyBCode">
              <el-input v-model="form.partyBCode" :disabled="isAll" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <el-form-item label="签订日期" prop="signingDate">
              <el-date-picker
                v-model="form.signingDate"
                type="date"
                placeholder="请选择签订日期"
                :disabled="isAll"
              />
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="备案日期" prop="filingDate">
              <el-date-picker
                v-model="form.filingDate"
                type="date"
                placeholder="请选择备案日期"
                :disabled="isAll"
              />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <el-form-item label="合同开始时间" prop="contractStartTime">
              <el-date-picker
                v-model="form.contractStartTime"
                type="date"
                placeholder="请选择合同开始日期"
                :disabled="isAll"
              />
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="合同结束时间" prop="contractEndTime">
              <el-date-picker
                v-model="form.contractEndTime"
                type="date"
                placeholder="请选择合同结束日期"
                :disabled="isAll"
              />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="22">
            <el-form-item label="备注" prop="remark">
              <el-input
                type="textarea"
                :rows="3"
                placeholder="请输入内容"
                v-model="form.remark"
                :disabled="isAll"
              />
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </div>
    <div class="add-contract-tables table">
      <form-label-title class="list">
        <span slot="title">
          合同扫码件上传
        </span>
      </form-label-title>
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
          prop="fileName"
          label="文件名称"
          width="270"
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
            <el-button v-show="!scope.row.fileUrl && !isAll" @click="handleClick(scope.row)" type="primary" size="small">上传</el-button>
            <el-button v-show="scope.row.fileUrl" type="primary" size="small">下载</el-button>
            <el-button v-show="scope.row.fileUrl" type="success" size="small">预览</el-button>
            <el-button v-show="scope.row.fileUrl && !isAll" type="danger" size="small">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    isAll: {
      type: Boolean,
      default: false
    }
  },
  data() {
    let validateEnrollEndTime = (rule, value, callback) => {
      if (
        value &&
        this.form.contractStartTime &&
        this.form.contractStartTime > value
      ) {
        callback(new Error('必须大于合同开始时间'))
      } else {
        callback();
      }
    };
    let validateEnrollStartTime = (rule, value, callback) => {
      if (
        value &&
        this.form.contractEndTime &&
        this.form.contractEndTime < value
      ) {
        callback(new Error('必须小于合同结束时间'));
      } else {
        callback();
      }
    };
    return {
      form: {
        contractName: '', // 合同名称
        contractAmount: '', // 合同金额
        partyAName: '', // 甲方名称
        partyBName: '', // 乙方名称
        partyACode: '', // 甲方代码
        partyBCode: '', // 乙方代码
        signingDate: '', // 签订日期
        filingDate: '', // 备案日期
        contractStartTime: '', // 合同开始时间
        contractEndTime: '', // 合同结束时间
        remark: '' // 备注
      },
      formRules: {
        contractName: [
          {
            required: true,
            message: '合同名称必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ],
        contractAmount: [
          {
            required: true,
            message: '合同金额必填',
            trigger: 'blur'
          },
          {
            pattern: /^[0-9]{0,}(\.[0-9]{0,})?$/,
            message: '只输入数字',
            trigger: 'blur'
          }
        ],
        partyAName: [
          {
            required: true,
            message: '甲方名称必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ],
        partyBName: [
          {
            required: true,
            message: '乙方名称必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ],
        partyACode: [
          {
            required: true,
            message: '甲方代码必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9]$/,
            message: '只能输入英文数字',
            trigger: 'blur'
          }
        ],
        partyBCode: [
          {
            required: true,
            message: '乙方代码必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9]$/,
            message: '只能输入英文数字',
            trigger: 'blur'
          }
        ],
        signingDate: [
          {
            required: true,
            message: '签订日期必填',
            trigger: 'blur'
          }
        ],
        filingDate: [
          {
            required: true,
            message: '备案日期必填',
            trigger: 'blur'
          }
        ],
        contractStartTime: [
          {
            required: true,
            message: '合同开始时间必填',
            trigger: 'blur'
          },
          {
            validator: validateEnrollStartTime,
            trigger: 'blur'
          }
        ],
        contractEndTime: [
          {
            required: true,
            message: '合同结束时间必填',
            trigger: 'blur'
          },
          {
            validator: validateEnrollEndTime,
            trigger: 'blur'
          }
        ],
        remark: [
          {
            required: true,
            message: '备注必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ]
      },
      tableData: [
        {
          fileName: '合同扫码件',
          updateTime: '2021-09-22',
          fileUrl: ''
        }
      ]
    }
  },
  methods: {
    checkRule() {

    },
    resetForm() {

    }
  }
}
</script>
<style scoped lang="scss">
@import '../../declar/assets/tables.scss';
.add-contract {
  margin-top:10px;
  .add-contract-from {
    ::v-deep .el-date-editor.el-input {
      width: 100%;
    }
  }
  .add-contract-tables {
    margin: 10px 0;
  }
}
</style>
