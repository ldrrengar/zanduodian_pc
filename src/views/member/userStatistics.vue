<template>
  <el-card class="box-card el-card-flex">
    <div slot="header" class="clearfix">
      <span>
        <i class="el-icon-position"></i>
         用户统计
      </span>
    </div>
    <div class="text item">
      <el-container style="overflow: hidden;">
        <el-main class="el-box">
          <el-tabs v-model="activeName" @tab-click="handleClick" class="el-tabs-bg el-flex el-tabs-flex approveTabs">
            <el-tab-pane :label="totalNum1" name="first" class="el-card-flex">
              <div class="el-flex el-box-column " style="overflow: auto">
                <el-table :data="tableData" ref="tableData" style="width: 100%;" border stripe>
                  <el-table-column show-overflow-tooltip align="center" type="index" width="50" label="序号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="username" label="账号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="name" label="昵称">
                  </el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="member_level" label="会员等级"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" prop="task_reward" align="center" label="任务奖励"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="commission" label="套餐提成"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="team_income" label="团队收益"></el-table-column>
<!--                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time_type_name" label="今日收益"></el-table-column>-->
<!--                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time_type_name" label="提现"></el-table-column>-->
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="balance" label="账户余额"></el-table-column>
                </el-table>
                <!-- 分页表格 -->
                <el-pagination background layout="total, sizes, prev, pager, next, jumper" :current-page="currentPage"
                               :page-size="pageSize" :total="total" @size-change="handleSizeChange" @current-change="handleCurrentChange"></el-pagination>
              </div>
            </el-tab-pane>
            <el-tab-pane :label="totalNum2" name="second" class="el-card-flex">
              <div class="el-flex el-box-column ">
                <el-table :data="tableData1" ref="tableData1" style="width: 100%;" border stripe>
                  <el-table-column show-overflow-tooltip align="center" type="index" width="50" label="序号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="username" label="账号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="name" label="昵称">
                  </el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="member_level" label="会员等级"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" prop="task_reward" align="center" label="任务奖励"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="commission" label="套餐提成"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="team_income" label="团队收益"></el-table-column>
                  <!--                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time_type_name" label="今日收益"></el-table-column>-->
                  <!--                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time_type_name" label="提现"></el-table-column>-->
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="balance" label="账户余额"></el-table-column>
                </el-table>
                <!-- 分页表格 -->
                <el-pagination background layout="total, sizes, prev, pager, next, jumper" :current-page="currentPage1"
                               :page-size="pageSize1" :total="total1" @size-change="handleSizeChange1" @current-change="handleCurrentChange1"></el-pagination>
              </div>
            </el-tab-pane>
          </el-tabs>
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
  name: 'userStatistics',
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
      total1: 0, // 总条数
      pageSize1: 20, // 每页展示条数
      currentPage1: 1, // 默认显示页数
      tableData1: [], // 查询列表
      dialogFormVisible: false,
      form: {
        reson: '',
        member: ''
      },
      activeName: 'first',
      formLabelWidth: '120px',
      isPass: false,
      rowData: '',
      totalAccount: '',
      toDayAccount: '',
      totalNum1: '',
      totalNum2: ''
    }
  },
  created() {
    this.getLists()
    this.getLists1()
    // this.totalNum1 = '总用户数量: ' + this.totalAccount
    // this.totalNum2 = '今日用户数量: ' + this.toDayAccount
  },
  methods: {
    // 总用户数获取事件
    getLists() {
      this.$http.get('/api/user/?page='+ this.currentPage + '&page_size=' + this.pageSize).then(res => {
        console.log(res)
        this.tableData = res.data.results
        this.total = res.data.count
        this.totalNum1 = '总用户数量: ' + res.data.count
      })
    },
    // 今日用户数获取事件
    getLists1() {
      this.$http.get('/api/user_today_add/?page='+ this.currentPage + '&page_size=' + this.pageSize).then(res => {
        console.log(res)
        this.tableData1 = res.data.results
        this.total1 = res.data.count
        this.totalNum2 = '今日新增用户数量: ' + res.data.count
      })
    },
    // tab栏点击事件
    handleClick(tab, event) {
    },
    // 总 每页条数
    handleSizeChange(val) {
      this.pageSize = val
      this.getLists()
    },
    // 总 当前页数
    handleCurrentChange(val) {
      this.currentPage = val
      this.getLists()
    },
    // 今日 每页条数
    handleSizeChange1(val) {
      this.pageSize1 = val
      this.getLists1()
    },
    // 今日 当前页数
    handleCurrentChange1(val) {
      this.currentPage1 = val
      this.getLists1()
    }
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
