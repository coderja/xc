<template>
  <div class="add-contract table">
    <div class="add-contract-from">
      <el-form ref="form" :model="form" label-width="140px" :rules="formRules">
        <el-row>
          <el-col :span="10">
            <el-form-item label="操作系统" prop="system">
              <el-input v-model="form.contractName" />
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="数据库类型" prop="dataType">
              <el-input v-model="form.contractAmount" />
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <el-form-item label="CPU数量" prop="cpuNum">
              <el-input v-model="form.cpuNum">
                  <template slot="append">个</template>
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="硬盘大小" prop="hardDisk">
              <el-input v-model="form.hardDisk">
                  <template slot="append">GB</template>
              </el-input>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="10">
            <el-form-item label="内存大小" prop="memory">
              <el-input v-model="form.memory">
                  <template slot="append">GB</template>
              </el-input>
            </el-form-item>
          </el-col>
          <el-col :span="10" :offset="2">
            <el-form-item label="是否需要互联网IP" prop="needIp">
              <el-radio-group v-model="form.needIp">
                    <el-radio :label="1">是</el-radio>
                    <el-radio :label="0">否</el-radio>
                </el-radio-group>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
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
        system: '', 
        dataType: '', 
        hardDisk: '', 
        cpuNum: '', 
        needIp: '', 
        memory: '', 
         
      },
      formRules: {
        system: [
          {
            required: true,
            message: '系统名称必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ],
        dataType: [
          {
            required: true,
            message: '数据库类型必填',
            trigger: 'blur'
          },
          {
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '只能输入中英文数字',
            trigger: 'blur'
          }
        ],
        memory: [
          {
            required: true,
            message: '内存大小必填',
            trigger: 'blur'
          },
          {
            pattern: /^[0-9]{0,}(\.[0-9]{0,})?$/,
            message: '只输入数字',
            trigger: 'blur'
          }
        ],
        hardDisk: [
          {
            required: true,
            message: '硬盘大小必填',
            trigger: 'blur'
          },
          {
            pattern: /^[0-9]{0,}(\.[0-9]{0,})?$/,
            message: '只输入数字',
            trigger: 'blur'
          }
        ],
        needIp: [
          {
            required: true,
            message: '是否需要互联网IP必选',
            trigger: 'change'
          },
          
        ],
        cpuNum: [
          {
            required: true,
            message: 'CPU数量必填',
            trigger: 'blur'
          },
          {
            pattern: /^[0-9]{0,}(\.[0-9]{0,})?$/,
            message: '只输入数字',
            trigger: 'blur'
          }
        ]
        
      },
       
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
