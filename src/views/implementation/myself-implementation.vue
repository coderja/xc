<template>
  <div class="myself-implementation">
    <!-- 筛选查询部分 -->
    <div class="myself-implementation-top">
      <el-row>
        <el-col :span="4">
          <el-date-picker
            v-model="years"
            type="year"
            placeholder="请选择实施年份"
          />
        </el-col>
        <el-col :span="4">
          <el-input
            v-model="keyword"
            resize="none"
            placeholder="请输入项目名称"
          />
        </el-col>
        <el-col :span="4">
          <el-select v-model="status" multiple placeholder="请选择">
            <el-option
              v-for="item in projectStatus"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
        </el-col>
        <el-button type="primary" round>查询</el-button>
        <el-button type="success" round>重置</el-button>
      </el-row>
    </div>
    <div class="myself-implementation-container">
      <div class="myself-implementation-left" :class="!isShowRight ? 'myself-implementation-left' : 'show-right'">
        <div class="main-container-table">
          <div class="operation">
            <i class="el-icon-menu" />
            <i class="el-icon-folder-checked" />
            <i class="el-icon-printer" />
          </div>
        </div>
        <div class="implementation-table">
          <el-table
            ref="multipleTable"
            :data="tableData"
            tooltip-effect="dark"
            style="width: 100%"
            @row-click="selectRow"
            @selection-change="handleSelectionChange">
            <el-table-column
              type="selection"
              width="55"
              align="center"
            />
            <el-table-column
              type="index"
              label="序号"
              width="120"
              align="center"
            />
            <el-table-column
              prop="name"
              label="项目名称"
              width="280"
              align="center"
            />
            <el-table-column
              prop="type"
              label="项目类型"
              width="280"
              align="center"
            />
            <el-table-column
              prop="softwareFunds"
              label="软件资金（万元）"
              width="150"
              align="center"
            />
            <el-table-column
              prop="hardwareFunds"
              label="硬件资金（万元）"
              width="150"
              align="center"
            />
            <el-table-column
              prop="serviceFunds"
              label="服务资金（万元）"
              width="150"
              align="center"
            />
            <el-table-column
              prop="operationMaintenanceFunds"
              label="运维资金（万元）"
              width="150"
              align="center"
            />
            <el-table-column
              prop="totalAmount"
              label="估算总金额（万元）"
              width="150"
              align="center"
            />
            <el-table-column
              prop="projectStatus"
              label="项目状态"
              width="150"
              align="center"
            />
            <el-table-column
              fixed="right"
              label="操作"
              align="center"
              width="100"
              @click.stop>
              <template slot-scope="scope">
                <el-button @click="clickItemMore(scope.row, scope.$index)" type="text" size="small" icon="el-icon-info"></el-button>
                <div class="table-operation" v-show="scope.row.isShow">
                  <div class="table-opertion-item" v-for="(item, index) in scope.row.tableOPertion" :key='index' @click="changeItemStatus(scope.row, item)">
                    {{item.label}}
                  </div>
                </div>
              </template>
            </el-table-column>
          </el-table>
          <div class="paging">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="current"
              :page-sizes="[10, 20, 30, 40]"
              :page-size=pageSize
              layout="total, sizes, prev, pager, next, jumper"
              :total=total
            />
          </div>
        </div>
      </div>
      <div class="myself-implementation-right" v-show="isShowRight">
        <div class="implementation-right-cancel" @click="isShowRight = false">
          <i class="el-icon-arrow-right"></i>
        </div>
        <div class="implementation-right-container">
          <div class="implementation-right-title">
            <div class="title-item" :class="tabBarIsShow === 'project-summary' ? 'active' : ''" @click="changeTabBar('project-summary')">
              <i class="el-icon-tickets" />
              <div class="title-item-text">项目概要</div>
            </div>
            <div class="title-item" :class="tabBarIsShow === 'audit-record' ? 'active' : ''" @click="changeTabBar('audit-record')">
              <i class="el-icon-document" />
              <div class="title-item-text">审核记录</div>
            </div>
            <div class="title-item" :class="tabBarIsShow === 'project-annex' ? 'active' : ''" @click="changeTabBar('project-annex')">
              <i class="el-icon-folder" />
              <div class="title-item-text">项目附件</div>
            </div>
          </div>
          <div class="implementation-right-content">
            <project-summary v-show="tabBarIsShow === 'project-summary'" />
            <audit-record :AuditRecord="AuditRecord" v-show="tabBarIsShow === 'audit-record'" />
            <project-annex v-show="tabBarIsShow === 'audit-record'" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import projectSummary from './components/project-summary.vue'
import auditRecord from './components/audit-record.vue'
import projectAnnex from './components/project-annex.vue'
export default {
  components: { projectSummary, auditRecord, projectAnnex },
  data() {
    return {
      years: '', // 选择的年份
      keyword: '', // 要搜索的值
      projectStatus: [
        {
          value: '0',
          label: '待汇总申报',
        },
        {
          value: '2',
          label: '2',
        }
      ],
      status: '',
      tableData: [
        {
          name: '项目全生命周期管理平台（十期）',
          type: '建设/新建/软件开发',
          softwareFunds: '200.00', // 软件资金
          hardwareFunds: '200.00', // 硬件资金
          serviceFunds: '0', // 服务资金
          operationMaintenanceFunds: '0', // 运维资金
          totalAmount: '200.00', // 估算总金额
          projectStatus: '待实施备案', // 项目状态
          tableOPertion: [
            {
              value: '0',
              label: '实施备案'
            }, {
              value: '1',
              label: '招投标备案'
            }, {
              value: '3',
              label: '合同备案'
            }, {
              value: '4',
              label: '项目跟踪'
            }, {
              value: '5',
              label: '项目变更'
            }, {
              value: '6',
              label: '验收申请'
            }, {
              value: '7',
              label: '验收备案'
            }
          ],
          isShow: false
        }, {
          name: '项目全生命周期管理平台（十期）',
          type: '建设/新建/软件开发',
          softwareFunds: '200.00', // 软件资金
          hardwareFunds: '200.00', // 硬件资金
          serviceFunds: '0', // 服务资金
          operationMaintenanceFunds: '0', // 运维资金
          totalAmount: '200.00', // 估算总金额
          projectStatus: '待实施备案', // 项目状态
          isShow: false
        }
      ],
      current: 1,
      total: 0,
      pageSize: 10,
      isShowRight: false,
      isShowAuditRecordMore: false,
      AuditRecord: [
        {
          name: '部门管理员审批已通过',
          status: '通过',
          time: '2021-09-12',
          opinion: 'hhhhhhhh',
          isShow: false
        }, {
          name: '部门管理员审批已通过',
          status: '通过',
          time: '2021-09-12',
          opinion: 'hhhhhhhh',
          isShow: false
        }
      ],
      tabBarIsShow: 'project-summary',
    }
  },
  methods: {
    handleSelectionChange(e) {
      console.log(e)
    },
    handleClick(e) {
      console.log(e)
    },
    /**
     * 点击分页触发的事件
     */
    handleCurrentChange(e) {
      console.log(e)
    },
    /**
     * 当前页要显示多少条数据
     */
    handleSizeChange(val) {
      console.log(val)
    },
    /**
     * 点击操作项
     */
    changeItemStatus(scope, item) {
      console.log(scope.row, item)
    },
    clickItemMore(item, i) {
      console.log(item, i)
      this.tableData[i].isShow = !this.tableData[i].isShow
    },
    /**
     * 当点击表格中某一行数据
     */
    selectRow(e) {
      console.log(e, '45454')
      this.isShowRight = true
    },
    changeTabBar(type) {
      this.tabBarIsShow = type
    }
    
  }
}
</script>
<style scoped lang="scss">
.myself-implementation {
  width: 100%;
  height: 100%;
  background: #eff0f2;
  position: absolute;
  top:0;
  left:0;
  padding: 15px;
  p {
    margin: 0;
    padding: 0;
  }
  .myself-implementation-top {
      width: 100%;
      height: 65px;
      background: #fff;
      padding: 15px 15px;
      ::v-deep .el-date-editor.el-input,
      .el-input,
      .el-select {
        width: 90%;
      }
    }
  .myself-implementation-container {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-between;
    .myself-implementation-left {
    width: 100%;
    &.show-right {
      width: calc(100% - 310px);
    }
    .main-container-table {
      width: 100%;
      display: flex;
      justify-content: flex-end;
      padding: 15px 15px;
      ::v-deep .el-icon-menu,
      .el-icon-folder-checked,
      .el-icon-printer {
        padding: 0 15px;
        font-size: 20px;
        color: #4380E8;
      }
      ::v-deep .el-icon-menu:before, .el-icon-folder-checked:before, .el-icon-printer:before {
        width: 20px;
        height: 20px;
      }
      .operation {
        background: #fff;
        border-radius: 35px;
        padding: 10px 12px;
        display: flex;
      }
    }
    .implementation-table {
      ::v-deep .el-icon-info {
        font-size: 20px;
      }
      .table-operation {
          max-width: 250px;
          min-width: 110px;
          padding: 5px 0;
          border: 1px solid #d2d2d2;
          overflow: visible;
          position: fixed;
          z-index:999999999;
          background: #fff;
          background: rgb(242, 242, 242);
          .table-opertion-item {
            padding: 0 24px 0 10px;
            font-size: 12px;
            cursor: pointer;
            &.table-opertion-item:hover {
              background: #1e9fff;
              color: #fff;
            }
          }

        }
      .paging {
        width:100%;
        height: 41px;
        padding: 7px 7px 0;
        font-size: 12px;
        background: #fff;
        margin-top: 30px;
      }
    }
  }
  .myself-implementation-right {
    width: 265px;
    height: 100%;
    position: relative;
    overflow: hidden;
    .implementation-right-cancel {
      width: 63px;
      height: 39px;
      background: #4180e8;
      margin: 15px;
      text-align: center;
      border-radius: 20px;
      line-height: 40px;
      margin-left: 200px;
      cursor: pointer;
      transform: translateX(0px);
      transition: 1s  all;
      i {
        color: #fff;
        font-size: 20px;
      }
    }
    .implementation-right-container {
      width: 100%;
      height: 100%;
      background: #fff;
      padding: 10px;
      .implementation-right-title {
      display: flex;
      border-bottom: 1px solid #edeef2;
      color: #bfbfbf;
      ::v-deep .el-icon-tickets, .el-icon-document, .el-icon-folder {
        font-size: 30px;
      }
      margin: 0 20px;
      .title-item {
        text-align: center;
        flex: 1;
        font-size: 14px;
        padding: 10px 0 8px 0;
        cursor: pointer;
        &.active {
          color: #4180e8;
          border-bottom: 3px solid #4180e8;
        }
      }
    }
    .implementation-right-content {
      .audit-record {
        .audit-record-item {
          padding: 10px 0;
          overflow: hidden;
          margin: 5px 0;
          position: relative;
          .item-status {
            font-size: 14px;
          }
          .item-flex {
            display: flex;
            justify-content: space-between;
            color: #999;
            margin-top: 5px;
            font-size: 14px;
          }
          .audit-record-item-more {
            padding: 6px 10px;
            color: #666;
            background: #eff0f2;
            margin: 10px 0;
            position: relative;
            z-index:9;
            line-height: 26px;
            font-size: 14px;
            .red {
              color: red;
            }
            &.audit-record-item-more::after {
              width: 0;
              height: 0;
              border-right: 10px solid transparent;
              border-left: 10px solid transparent;
              border-bottom: 10px solid #eff0f2;
              float: right;
              margin-right: 13px;
              position: absolute;
              content: "";
              top: -10px;
              right: 10px;
            }
          }
        }
      }
    }
    }
  }
  }
}
</style>
