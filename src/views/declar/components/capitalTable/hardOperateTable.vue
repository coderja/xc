<template>
  <div class="table">
    <form-label-title class="list">
      <span slot="title">
        硬件运维投资估算明细（万元）
      </span>
    </form-label-title>
    <div class="t-operate-btn">
      <span class="text-info">合计</span>
      <span class="text-info">{{ total }}</span>
      <el-button type="primary" size="small" v-if='!isDetail'>模板下载</el-button>
      <el-button type="primary" size="small" v-if='!isDetail'>数据导入</el-button>
      <el-button type="danger" size="small" v-if='!isDetail' @click='deleteTable'>删除</el-button>
    </div>
    <el-form ref="HOForm" :model="tableInfo" :disabled='isDetail' :rules="quotedInfRules">
      <el-table
        :data="tableInfo.tableDatas"
        style="width: 100%"
        :summary-method="getSummaries"
        show-summary
      >
        <el-table-column prop="plotNumber" width="60">
          <template slot="header" slot-scope="scope">
            <span>序号</span>
          </template>
          <template slot-scope="scope">
            {{ scope.$index }}
          </template>
        </el-table-column>

        <el-table-column label="设备名称" prop="sysName" width="100">
          <!-- <template slot="header">
            <span><i style="color: #f56c6c">*</i> 系统名称</span>
          </template> -->
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.sysName'"
            >
              <el-input v-model="scope.row.sysName" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="资产编号" prop="assetsNum" width="100">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.assetsNum'"
            >
              <el-input v-model="scope.row.assetsNum" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="采购时间" prop="purchaseDate" width="100">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.purchaseDate'"
            >
              <el-input v-model="scope.row.purchaseDate" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="过保时间" prop="WarrantyDate">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.WarrantyDate'"
            >
              <el-input v-model="scope.row.WarrantyDate" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="设备单价（万元）" prop="price">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.price'"
            >
              <el-input v-model="scope.row.price" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="数量" prop="number" width="100">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.number'"
            >
              <el-input v-model="scope.row.number" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="往期运维结束时间" prop="prevOperateEndDate" width="100">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.prevOperateEndDate'"
            >
              <el-input v-model="scope.row.prevOperateEndDate" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="往期运维费用（万元）" prop="prevOperateCost" width="100">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.prevOperateCost'"
            >
              <el-input v-model="scope.row.prevOperateCost" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column
          label="本期运维费用占比（运维费用/建设费用）"
          prop="nowOperateCostRatio"
          width="100"
        >
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.nowOperateCostRatio'"
            >
              <el-input v-model="scope.row.nowOperateCostRatio" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="本期运维费用（万元）" prop="nowOperateCost" width="100">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.nowOperateCost'"
            >
              <el-input v-model="scope.row.nowOperateCost" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="操作" width="100"  v-if='!isDetail' fixed='right'>
          <template slot-scope="scope">
            <el-button
              icon="el-icon-delete"
              title="删除"
              type="danger"
              size="mini"
              :disabled="canEdit"
              @click="deleteRow(scope.$index, scope.row)"
            />
          </template>
        </el-table-column>
      </el-table>
    </el-form>
  </div>
</template>

<script>
export default {
  props: {
    tableData: {
      type: Array
    },
    refenrence: {
      type: String
    },
    canEdit: {
      type: Boolean
    }
  },
  computed: {
    isDetail() {
      return this.$route.query.detail
    }
  },
  data() {
    const validateNum = (rule, value, callback) => {
      const regStr = /^(\d+)(\.\d+)?$/
      if (value && !regStr.test(value)) {
        callback(new Error('请输入数字'))
      } else {
        callback()
      }
    }
    return {
      tableInfo: {
        tableDatas: [
          {
            sysName: '1', // 系统名称
            assetsNum: '2', // 资产编号
            purchaseDate: '22', // 采购时间
            WarrantyDate: '22', // 过保时间
            price: '22', // 设备单价
            number: '22', // 数量
            prevOperateEndDate: '12', // 往期运维结束时间
            prevOperateCost: '12', // 往期运维费用
            nowOperateCostRatio: '222', // 本期运维费用占比
            nowOperateCost: '22222', // 本期运维费用
          }
        ]
      },
      total: '',
      quotedInfRules: {
        name: [
          {
            required: false,
            pattern: /^[A-Za-z0-9\u4e00-\u9fa5]{0,32}$/,
            message: '请输入中英文数字',
            trigger: 'blur'
          }
        ],

        limitHigh: [
          {
            required: false,
            // pattern: ptn.all_match(1, 40),
            message: '超出最大字符长度',
            trigger: 'blur'
          }
        ],

        number: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ]
      }
    }
  },
  watch: {},
  mounted() {},
  methods: {
    deleteTable() {
      this.$confirm('确实要删除当前表格的数据？', '删除提示', {
        confirmButtonText: '确认',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
          this.tableInfo.tableDatas = []
        })
    },
    deleteRow(i, row) {
      this.$confirm('确实要删除当前行？', '删除提示', {
        confirmButtonText: '确认',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
          this.tableInfo.tableDatas.splice(i, 1)
        })
    },
    getSummaries(param) {
      var _this = this
      const { columns, data } = param
      const sums = []

      console.log(param)
      columns.forEach((column, index) => {
        if (index === 0) {
          sums[index] = '合计'
          return
        }
        const values = data.map(item => {
          const fileds = [
            'sysName',
            'assetsNum',
            'purchaseDate',
            'WarrantyDate',
            'price',
            'number',
            'prevOperateEndDate',
            'prevOperateCost',
            'nowOperateCostRatio',
            'nowOperateCost'
          ]
          if (fileds.indexOf(column.property) !== -1) {
            if (column.property == 'total') {
              return Number(item[column.property])
            }
          }
        })

        if (
          !values.every(value => isNaN(value)) &&
          column.property === 'total'
        ) {
          sums[index] = values.reduce((prev, curr) => {
            const value = Number(curr)
            if (!isNaN(value)) {
              return prev + curr
            } else {
              return prev
            }
          }, 0)
          _this.$emit('sendTotal', (_this.total = sums[8]))
        } else {
          sums[index] = ''
        }
      })

      return sums
    }
  }
}
</script>

<style scoped lang="scss">
@import '../../assets/tables.scss';
.cell .el-form-item {
  margin-bottom: 0px;
}
</style>
