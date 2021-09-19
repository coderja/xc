<template>
  <div>
    <el-form :model="tableInfo" ref="tableForm" :rules="quotedInfRules">
      <el-table
        :data="tableInfo.tableDatas"
        style="width: 100%"
        border
        :summary-method="getSummaries"
        show-summary
      >
        <el-table-column fixed="left" prop="plotNumber" width="200">
          <template slot="header" slot-scope="scope">
            <span>序号</span>
          </template>
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.plotNumber"
              :prop="'tableDatas.' + scope.$index + '.plotNumber'"
            >
              <el-input v-model="scope.row.plotNumber" disabled></el-input>
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column fixed="left" prop="landUse" width="100">
          <template slot="header" slot-scope="scope">
            <span><i style="color: #f56c6c">*</i> 系统名称</span>
          </template>
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.landUse"
              :prop="'tableDatas.' + scope.$index + '.landUse'"
            >
               <el-input></el-input>
            </el-form-item>
          </template>
        </el-table-column>

        <el-table-column label='子系统名称（如有）' prop="landUse" width="100">
          <template slot-scope="scope">
            <el-form-item
              :rules="quotedInfRules.landUse"
              :prop="'tableDatas.' + scope.$index + '.landUse'"
            >
               <el-input></el-input>
            </el-form-item>
          </template>
        </el-table-column>
       
        <el-table-column label="操作" width="100">
          <template slot-scope="scope">
            <el-button
              type="text"
              icon="el-icon-delete"
              title="删除"
              :disabled="!canEdit"
              @click="deleteRow(scope.$index, scope.row)"
            ></el-button>
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
  data() {
    let validateNum = (rule, value, callback) => {
      let regStr = /^(\d+)(\.\d+)?$/;
      if (value && !regStr.test(value)) {
        callback(new Error('请输入数字'));
      } else {
        callback();
      }
    };
    let validatePer = (rule, value, callback) => {
      let regStr = /^(\d+)(\.\d+)?$/;
      if (value && (!regStr.test(value) || value > 100)) {
        callback(new Error('请输入数字'));
      } else {
        callback();
      }
    };
    let validateNoticeTime = (rule, value, callback) => {
      if (
        value &&
        this.quotedInf.signupDeadline &&
        this.quotedInf.signupDeadline < value
      ) {
        callback(new Error('必须小于报名截止时间'));
      } else {
        callback();
      }
    };
    let validateSignupline = (rule, value, callback) => {
      if (
        value &&
        this.quotedInf.noticeTime &&
        this.quotedInf.noticeTime > value
      ) {
        callback(new Error('必须大于公告时间'));
      } else if (
        value &&
        this.quotedInf.dealDeadline &&
        this.quotedInf.dealDeadline < value
      ) {
        callback(new Error('必须小于出让截止时间'));
      } else {
        callback();
      }
    };
    let validateDealline = (rule, value, callback) => {
      if (
        value &&
        this.quotedInf.signupDeadline &&
        this.quotedInf.signupDeadline > value
      ) {
        callback(new Error('必须大于报名截止时间'));
      } else {
        callback();
      }
    };
    return {
      
      totalBusinessArea: 0, //总商业建筑面积
      totalConstructionArea: 0, //
      totalConstructionAreaMu: 0,
      totalHouseArea: 0,
      totalPlanConstructionArea: 0,
      totalPlotTotalArea: 0,
      capacityBuildingArea: 0,
      quotedInfRules: {
        plotNumber: [
          {
            required: false,
            pattern: ptn.cn_en_num_cen_line(1, 20),
            message: '请输入中英文数字',
            trigger: 'blur'
          }
        ],
        landUse: [
          {
            required: true,
            message: '请选择',
            trigger: 'change'
          }
        ],
        plotTotalArea: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字' }
        ],
        constructionArea: [
          {
            required: false,
            message: '请输入数字',
            trigger: 'blur'
          },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ],
        constructionAreaMu: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ],
        plotRatio: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ],
        planConstructionArea: [
          { required: true, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ],
        houseArea: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ],
        businessArea: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ],
        limitHigh: [
          {
            required: false,
            pattern: ptn.all_match(1, 40),
            message: '超出最大字符长度',
            trigger: 'blur'
          }
        ],
        buildingDensity: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ],
        greenRate: [
          { required: false, message: '请输入数字', trigger: 'blur' },
          { validator: validateNum, message: '请输入数字', trigger: 'blur' }
        ]
      }
    };
  },
  methods: {
    deleteRow(i, row) {
      this.$confirm('确实要删除当前行？', '删除提示', {
        confirmButtonText: this.$l('btn_confirm'),
        cancelButtonText: this.$l('btn_cancel'),
        type: 'warning'
      }).then(() => {
        delLandPlotById({ id: row.id }).then(() => {
          this.$message({
            message: '删除成功',
            type: 'success'
          });
          this.$root.Bus.$emit('reloadMap');
          this.tableInfo.tableDatas.splice(i, 1);
        });
      });
    },
    getSummaries(param) {
      var _this = this;
      const { columns, data } = param;
      const sums = [];

      columns.forEach((column, index) => {
        if (index === 0) {
          sums[index] = '合计';
          return;
        }
        const values = data.map(item => {
          let fileds = [
            'plotTotalArea',
            'businessArea',
            'constructionArea',
            'constructionAreaMu',
            'houseArea',
            'planConstructionArea'
          ];
          if (fileds.indexOf(column.property) !== -1) {
            if (column.property == 'planConstructionArea') {
              return (
                Number(item['constructionArea']) * Number(item['plotRatio'])
              );
            }
            return Number(item[column.property]);
          }
        });

        if (!values.every(value => isNaN(value))) {
          sums[index] = values.reduce((prev, curr) => {
            const value = Number(curr);
            if (!isNaN(value)) {
              return prev + curr;
            } else {
              return prev;
            }
          }, 0);
          sums[index] += '';
          if (index == 4) {
            sums[index] = (sums[3] * 0.0015).toFixed(2);
          }
          if (index == 6) {
            sums[index] = (sums[index] * 1).toFixed(2);
          }

          switch (column.property) {
            case 'businessArea':
              _this.totalBusinessArea = sums[index];
              break;
            case 'constructionArea':
              _this.totalConstructionArea = sums[index];
              break;
            case 'constructionAreaMu':
              _this.totalConstructionAreaMu = sums[index];
              break;
            case 'houseArea':
              _this.totalHouseArea = sums[index];
              break;
            case 'planConstructionArea':
              _this.totalPlanConstructionArea = sums[index];
              break;
            case 'plotTotalArea':
              _this.totalPlotTotalArea = sums[index];
              break;
          }
        } else {
          sums[index] = '';
        }
      });

      return sums;
    }
  },
  watch: {
    
  },
  mounted() {}
};
</script>

<style scoped></style>
