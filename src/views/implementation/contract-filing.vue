<template>
  <div class="contract-filing">
    <!-- 筛选查询部分 -->
    <div class="contract-filing-top">
      <el-row>
        <el-col :span="4">
          <el-input
            v-model="contractName"
            resize="none"
            placeholder="请输入合同名称"
          />
        </el-col>
        <el-col :span="4">
          <el-select v-model="contractType" multiple placeholder="请选择合同类型">
            <el-option
              v-for="item in contractTypeList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-col>
        <el-col :span="4">
          <el-input
            v-model="partyBName"
            resize="none"
            placeholder="请输入乙方名称"
          />
        </el-col>
        <el-button type="primary">查询</el-button>
        <el-button type="success" @click="reset">重置</el-button>
      </el-row>
    </div>
    <div class="contract-filing-operation" v-if="!isAll">
      <el-button type="primary" @click="isShowContract = true">新建</el-button>
      <el-button type="danger" @click="detele">删除</el-button>
    </div>
    <div class="contract-filing-table">
      <el-table
        :data="tableData"
        style="width: 100%"
        @selection-change="handleSelectionChange">
        <el-table-column
          type="selection"
          width="55"
        />
        <el-table-column
          prop="contractName"
          label="合同名称"
          width="420"
          align="center"
          show-overflow-tooltip
        />
        <el-table-column
          prop="contractType"
          label="合同类型"
          width="320"
          align="center"
        />
        <el-table-column
          prop="partyAName"
          label="甲方名称"
          width="280"
          align="center"
          show-overflow-tooltip
        />
        <el-table-column
          prop="partyBName"
          label="乙方名称"
          show-overflow-tooltip
          width="350"
          align="center"
        />
        <el-table-column
          prop="contractSigningDate"
          label="合同签订日期"
          show-overflow-tooltip
          width="250"
          align="center"
        />
        <el-table-column
          fixed="right"
          label="操作"
          align="center"
          width="100">
          <template slot-scope="scope">
            <el-button @click="clickItemMore(scope.row, scope.$index)" type="primary" size="small">{{isAll ? '查看' : '编辑'}}</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <el-dialog
      class="dialog"
      top="5%"
      :append-to-body="true"
      width="50%"
      :title="isAll ? '查看合同' : '新增合同'"
      :visible.sync="isShowContract"
      @open="openDialog"
    >
      <add-contract ref="addContract" :isAll="isAll" />
      <div class="contract-btn" style="text-align:center">
        <el-button type="info" @click="cancel">返回</el-button>
        <el-button v-show="!isAll" type="primary" @click="confirmForm">保存</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
import addContract from './components/add-contract.vue'
export default {
  components: { addContract },
  data() {
    return {
      contractTypeList: [{
        value: '',
        label: '请选择合同类型'
      }, {
        value: '1',
        label: '项目合同'
      }, {
        value: '2',
        label: '监理合同'
      }, {
        value: '3',
        label: '软测合同'
      }],
      contractType: '',
      contractName: '', // 合同名称
      partyBName: '', // 乙方名称
      tableData:[ // 合同数据
        {
          contractName: '项目全生命周期管理平台（时期）项目合同',
          contractType: '项目合同',
          partyAName: '苏州市卫生健康委员会',
          partyBName: '航天软测',
          contractSigningDate: '2021-09-22',
        }
      ],
      isShowContract: false, // 是否显示新增/编辑合同
      selectRows:[],
      isAll: false,
    }
  },
  created() {
    this.isAll = this.$route.query.type === 'all'
  },
  methods: {
    handleSelectionChange(rows) {
      this.selectRows = rows
    },
    /**
     * 是否要删除的二次确认框
     */
    detele() {
      if (this.selectRows.length === 0) {
        this.$message({
          type: 'warning',
          message: '请选择需要删除的行!'
        })
        return
      }
      this.$confirm('是否确认删除？', '确认删除', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        //this.selectRows 调接口传的参数
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
      })
    },
    openDialog() {
      this.$nextTick(() => {})
    },
    /**
     * 弹框的返回事件
     */
    cancel() {
      this.isShowContract = false
      this.$refs.addContract.$refs.form.resetFields()
    },
    confirmForm() {
      
    },
    reset() {
      this.contractName = ''
      this.contractType = ''
    },
    clickItemMore() {
      this.isShowContract = true
    }
  }
}
</script>
<style scoped lang="scss">
.contract-filing {
  width: 100%;
  height: 100%;
  background: #eff0f2;
  position: absolute;
  top: 0;
  left: 0;
  .contract-filing-top {
      width: 100%;
      height: 65px;
      background: #fff;
      padding: 15px 15px;
      ::v-deep .el-input,.el-select {
        width: 90%;
      }
    }
    .contract-filing-operation {
      padding: 15px;
      display: flex;
    }
    .contract-filing-table {
      padding: 0 15px;
    }
}
::v-deep .el-dialog__header{
      background: #409EFF;
      padding: 20px;
      .el-dialog__title {
        font-size: 25px;
        color: #fff;
      }
    }
.contract-btn {
  ::v-deep .el-button--primary, .el-button--info {
    padding: 12px 33px;
}
}
::v-deep  .el-dialog__close {
  &:hover {
    color: #fff;
    font-size: 18px;
  }
  color: #909399;
  color: #fff;
  font-size: 18px;
}
</style>
