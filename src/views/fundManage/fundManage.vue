<template>
  <el-card class="box-card el-card-flex">
    <div slot="header" class="clearfix">
      <span>
        <i class="el-icon-position"></i>
         资金明细统计
      </span>
    </div>
    <div class="text item">
      <el-container style="overflow: hidden;">
        <el-main class="el-box">
          <div class="el-flex el-box-column ">
            <el-table :data="tableData" ref="tableData" style="width: 100%;" border stripe>
              <el-table-column show-overflow-tooltip align="center" type="index" width="50" label="序号"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="value1" label="资金明细"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="value2" label="发布任务充值">
                <template  slot-scope="scope">
                  <div @click="nextPage(1, scope.row.value0)" style="color: #0b82e9; cursor: pointer;">{{ scope.row.value2 }}</div>
                </template>
              </el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="value3" label="会员充值">
                <template  slot-scope="scope">
                  <div @click="nextPage(2, scope.row.value0)" style="color: #0b82e9; cursor: pointer;">{{ scope.row.value2 }}</div>
                </template>
              </el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="value4" label="套餐提成"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="value5" label="任务奖励"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="value6" label="团队收益"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="120"  prop="value7" align="center" label="提现">
                <template  slot-scope="scope">
                  <div @click="nextPage(3, scope.row.value0)" style="color: #0b82e9; cursor: pointer;">{{ scope.row.value2 }}</div>
                </template>
              </el-table-column>
            </el-table>
            <!--&lt;!&ndash; 分页表格 &ndash;&gt;-->
            <!--<el-pagination background layout="total, sizes, prev, pager, next, jumper" :current-page="currentPage"-->
                           <!--:page-size="pageSize" :total="total" @size-change="handleSizeChange" @current-change="handleCurrentChange"></el-pagination>-->
          </div>
        </el-main>
      </el-container>
    </div>
  </el-card>
</template>
<script>
import {
  Tree,
  Dialog,
  Table,
  TableColumn,
  Pagination,
  MessageBox,
  Message,
  Image,
  Drawer,
  Card,
  TabPane,
  Tabs
} from 'element-ui'
export default {
  name: 'taskFund',
  components: {
    'el-table': Table, // 表格
    'el-table-column': TableColumn, // 表格列
    'el-pagination': Pagination, // 分页
    'el-dialog': Dialog, // 对话框
    'el-drawer': Drawer, // 抽屉
    'el-image': Image, // 抽屉
    'el-card': Card, // 卡片
    'el-tab-pane': TabPane, // 导航
    'el-tabs': Tabs
  },
  data() {
    return {
      total: 0, // 总条数
      pageSize: 20, // 每页展示条数
      currentPage: 1, // 默认显示页数
      tableData: [], // 查询列表
      dialogFormVisible: false,
      form: {
        reson: '',
        member: ''
      },
      activeName: 'first',
      formLabelWidth: '120px',
      isPass: false,
      rowData: ''
    }
  },
  created() {
    this.getLists()
  },
  methods: {
    // 总用户数获取事件
    getLists() {
      this.tableData = [
        {
          value0: 'day',
          value1: '今日',
          value2: '15',
          value3: '20',
          value4: '80',
          value5: '11',
          value6: '23',
          value7: '33'
        },
        {
          value0: 'month',
          value1: '本月',
          value2: '15',
          value3: '20',
          value4: '80',
          value5: '11',
          value6: '23',
          value7: '33'
        },
        {
          value0: 'total',
          value1: '总金额',
          value2: '15',
          value3: '20',
          value4: '80',
          value5: '11',
          value6: '23',
          value7: '33'
        }
      ]
    },
    nextPage(type, val) {
      if (type === 1) {
        this.$router.push({
          path: '/taskFund',
          query: {
            fundType: val
          }
        })
      } else if (type === 2) {
        this.$router.push({
          path: '/getMember',
          query: {
            fundType: val
          }
        })
      } else if (type === 3) {
        this.$router.push({
          path: '/getMoney',
          query: {
            fundType: val
          }
        })
      }
    }
    // // 总 每页条数
    // handleSizeChange(val) {
    //   this.pageSize = val
    //   this.getLists()
    // },
    // // 总 当前页数
    // handleCurrentChange(val) {
    //   this.currentPage = val
    //   this.getLists()
    // }
  }
}
</script>
<style scoped lang="scss">
  .box-card {
    height: 100%;
  }
  .el-card__body {
    height: 100%;
  }
  .el-table {
    th {
      padding: 6px 0;
    }

    td {
      padding: 6px 0;
    }
  }

  .el-table__row {
    .cell {
      a {
        color: #409eff;
      }
    }
  }

  .elAdded .el-date-editor.el-input,
  .el-date-editor.el-input__inner {
    width: 190px;
  }

  .display {
    display: none;
  }
  .el-main {
    padding: 0 !important;
  }
</style>
