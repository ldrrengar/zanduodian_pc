<template>
  <el-card class="box-card el-card-flex">
    <div slot="header" class="clearfix">
      <span>
        <i class="el-icon-position"></i>
         任务信息统计
      </span>
    </div>
    <div class="text item">
      <el-container style="overflow: hidden;">
        <el-main class="el-box">
          <el-tabs v-model="activeName" @tab-click="handleClick" class="el-tabs-bg el-flex el-tabs-flex approveTabs">
            <el-tab-pane :label="totalNum1" name="first" class="el-card-flex">
              <div class="el-flex el-box-column ">
                <el-table :data="tableData" ref="tableData" style="width: 100%;" border stripe>
                  <el-table-column show-overflow-tooltip align="center" type="index" width="50" label="序号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="created" label="任务编号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="created" label="视频链接"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="member_level_name" label="目标次数">
                  </el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="money" label="已完成次数"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="money" label="任务要求"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" prop="state" align="center" label="发布任务价格"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="time" label="单条完成价格"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time" label="费用总计"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="time" label="状态"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time" label="任务类型"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="time" label="发布人昵称"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="time" label="发布人账号"></el-table-column>
                </el-table>
                <!-- 分页表格 -->
                <el-pagination background layout="total, sizes, prev, pager, next, jumper" :current-page="currentPage"
                               :page-size="pageSize" :total="total" @size-change="handleSizeChange" @current-change="handleCurrentChange"></el-pagination>
              </div>
            </el-tab-pane>
            <el-tab-pane :label="totalNum2" name="second" class="el-card-flex">
              <div class="el-flex el-box-column">
                <el-table :data="tableData1" ref="tableData1" style="width: 100%;" border stripe>
                  <el-table-column show-overflow-tooltip align="center" type="index" width="50" label="序号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="created" label="任务编号"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="created" label="视频链接"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="member_level_name" label="目标次数">
                  </el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="money" label="已完成次数"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="money" label="任务要求"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" prop="state" align="center" label="发布任务价格"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="time" label="单条完成价格"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time" label="费用总计"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="time" label="状态"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="100" align="center" prop="time" label="任务类型"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="time" label="发布人昵称"></el-table-column>
                  <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="time" label="发布人账号"></el-table-column>
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
  name: 'totalTask',
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
      totalTask: '11',
      toDayTask: '12',
      totalNum1: '',
      totalNum2: ''
    }
  },
  created() {
    this.getLists()
    this.getLists1()
    this.totalNum1 = '总任务数量: ' + this.totalTask // 包括已完成
    this.totalNum2 = '今日新增数量: ' + this.toDayTask // 包括已完成
  },
  methods: {
    // 总用户数获取事件
    getLists() {
    },
    // 今日用户数获取事件
    getLists1() {
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
