<template>
  <el-card class="box-card el-card-flex">
    <div slot="header" class="clearfix">
      <span>
        <i class="el-icon-position"></i>
         提现
      </span>
    </div>
    <div class="text item">
      <el-container style="overflow: hidden;">
        <el-main class="el-box">
          <div class="el-flex el-box-column ">
            <el-table :data="tableData" ref="tableData" style="width: 100%; overflow: auto;" border stripe>
              <el-table-column show-overflow-tooltip align="center" type="index" width="50" label="序号"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="120" align="center" prop="ZFB_account" label="账户">
              </el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="ZFB_name" label="账户名"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="money" label="提现金额"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="add_time" label="提现时间"></el-table-column>
              <el-table-column show-overflow-tooltip sortable min-width="80" align="center" prop="state" label="提现状态"></el-table-column>
            </el-table>
            <!-- 分页表格 -->
            <el-pagination background layout="total, sizes, prev, pager, next, jumper" :current-page="currentPage"
                           :page-size="pageSize" :total="total" @size-change="handleSizeChange" @current-change="handleCurrentChange"></el-pagination>
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
  name: 'getMember',
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
      this.$http.get('/api/money_record_look/?page='+ this.currentPage + '&page_size=' + this.pageSize + '&type=' + this.$route.query.fundType).then(res => {
        console.log(res)
        this.tableData = res.data.results
        this.total = res.data.count
      })
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
