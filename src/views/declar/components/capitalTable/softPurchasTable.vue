<template>
  <div class="table">
    <form-label-title class="list">
      <span slot="title">
        软件采购类项目投资估算明细（万元）
      </span>
    </form-label-title>
    <div class="t-operate-btn">
      <span class="text-info">合计</span>
      <span class="text-info">{{ total }}</span>
      <el-button type="primary" size="small" v-if='!isDetail'>模板下载</el-button>
      <el-button type="primary" size="small" v-if='!isDetail'>数据导入</el-button>
      <el-button type="danger" size="small" v-if='!isDetail' @click='deleteTable'>删除</el-button>
    </div>
    <el-form ref="SPForm" :model="tableInfo" :disabled='isDetail' :rules="quotedInfRules">
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

        <el-table-column label="系统名称" prop="sysName" width="100">
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

        <el-table-column label="功能模块名称" prop="funcName" width="120">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.funcName'"
            >
              <el-input v-model="scope.row.funcName" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="功能描述（30-200字）" prop="funcDesc">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.funcDesc'"
            >
              <el-input v-model="scope.row.funcDesc" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="市场调研说明" prop="markRepExplain">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.name"
              :prop="'tableDatas.' + scope.$index + '.markRepExplain'"
            >
              <el-input v-model="scope.row.markRepExplain" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="单价（万元/月）" prop="price" width="140">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.price'"
            >
              <el-input v-model="scope.row.price" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="数量（人月）" prop="number" width="120">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.number'"
            >
              <el-input v-model="scope.row.number" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label="小计（万元）" prop="total" width="120">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.number"
              :prop="'tableDatas.' + scope.$index + '.total'"
            >
              <el-input v-model="scope.row.total" />
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column v-if='!isDetail' label="操作" width="100" fixed='right'>
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
            funcName: '22', // 功能模块名称
            funcDesc: '21', // 功能描述
            markRepExplain: '12', // 市场调研说明
            price: '2', // 单价
            number: '3', // 数量
            total: '' // 小计
          },
          {
            sysName: '1', // 系统名称
            funcName: '22', // 功能模块名称
            funcDesc: '21', // 功能描述
            markRepExplain: '12', // 市场调研说明
            price: '2', // 单价
            number: '', // 数量
            total: '' // 小计
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
  watch: {
    'tableInfo.tableDatas':{
      handler() {
        this.tableInfo.tableDatas.map(el=>{
          if(el.price && el.number && !isNaN(el.price)&&!isNaN(el.number)) {
            return el.total = el.price * el.number
          }
        })
      },
      immediate: true
    } 

  },
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
            'funcName',
            'funcDesc',
            'markRepExplain',
            'price',
            'number',
            'total'
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
