<template>
  <div class="goods">
    <!--1. 搜索区域-->
    <div class="header">
      <el-input @change="searchInput" v-model="input" placeholder="请输入内容"></el-input>
      <el-button type="primary">查询</el-button>
      <el-button type="primary" @click="toAddGoods">页面添加</el-button>
      <el-button type="primary" @click="addGoods">弹窗添加</el-button>
    </div>
    <!--2. 表格区域展示视图数据-->
    <div class="wrapper">
      <el-table
        :data="tableData"
        border
        v-loading="loading"
        style="width: 100%">
        <el-table-column type="selection" width="55"></el-table-column>
        <el-table-column prop="id" label="商品ID" width="100"></el-table-column>
        <el-table-column prop="title" label="商品名称" width="100" show-overflow-tooltip></el-table-column>
        <el-table-column prop="price" label="商品价格" width="100"></el-table-column>
        <el-table-column prop="num" label="商品数量" width="100"></el-table-column>
        <el-table-column prop="category" label="规格类目" width="100"></el-table-column>
        <el-table-column prop="image" label="商品图片" show-overflow-tooltip></el-table-column>
        <el-table-column prop="sellPoint" label="商品卖点" show-overflow-tooltip></el-table-column>
        <el-table-column prop="descs" label="商品描述" show-overflow-tooltip></el-table-column>
        <el-table-column label="操作" width="180">
          <template slot-scope="scope">
            <el-button
              size="mini"
              @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
            <el-button
              size="mini"
              type="danger"
              @click="handleDelete(scope.$index, scope.row)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>
    <!--3. 分页-->
    <MyPagination :total="total" :pageSize="pageSize" @changePage="changePage" :currentPage="currentPage"></MyPagination>
    <!--弹窗-->
<!--    <AddGoodsDialog :dialogVisible="dialogVisible" @handleClose="handleClose" />-->
    <AddGoodsDialog ref="dialog" :title="dialogTitle" :formData="rowData" />

  </div>
</template>

<script>
import MyPagination from '../../components/MyPagination'
import AddGoodsDialog from '@/views/Goods/AddGoodsDialog'
export default {
  name: 'Goods',
  components: { AddGoodsDialog, MyPagination },
  data () {
    return {
      dialogTitle: '添加商品',
      rowData: {},
      input: '',
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }],
      total: 0,
      pageSize: 8,
      currentPage: 1, // 当前页码
      loading: false,
      type: 1, // 1-商品列表； 2-搜索
      list: []
    }
  },
  methods: {
    addGoods() {
      // this.dialogVisible = true
      this.$refs.dialog.dialogVisible = true
      this.dialogTitle = '添加商品'
    },
    handleClose() {
      this.dialogVisible = false
    },
    handleEdit (index, row) {
      console.log('编辑', index, row)
      // 1. 点击编辑按钮 显示弹窗
      this.$refs.dialog.dialogVisible = true
      this.dialogTitle = '编辑商品'
      this.rowData = { ...row } // 不能传引用 必须拷贝过去才不会出问题
      // 2. 数据回显
    },
    handleDelete (index, row) {
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        // 请求接口
        console.log('删除', index, row)
        this.$api.deleteGoods({ id: row.id }).then(res => {
          if(res.status === 200) {
            this.getGoodsList(this.currentPage)
            this.$message({
              type: 'success',
              message: '删除成功!'
            })
          } else {
            this.$message.error('删除错误')
          }
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    /**
     * 查询数据
     */
    searchInput () {
      const value = this.input
      if (!value) {
        this.getGoodsList(1)
        this.type = 1
        return
      }
      this.$api.getSearch({
        search: value
      }).then(res => {
        this.currentPage = 1
        if (res.status === 200) {
          // this.tableData = res.result
          this.list = res.result
          this.total = res.result.length
          // this.pageSize = this.pageSize
          this.tableData = res.result.slice(0, this.pageSize)
          this.type = 2
        } else {
          this.tableData = []
        }
      })
    },
    /**
     * 获取商品列表
     * @param page
     */
    getGoodsList (page) {
      this.loading = true
      this.$api.getGoodsList({ page }).then(res => {
        this.loading = false
        if (res.status === 200) {
          this.tableData = res.data
          this.total = res.total
          this.pageSize = res.pageSize
        } else {
          this.tableData = []
          this.total = 0
          this.pageSize = 1
        }
      })
    },
    changePage (page) {
      this.currentPage = page
      if (this.type === 1) {
        this.getGoodsList(page)
      } else {
        console.log('处理分页')
        this.tableData = this.list.slice((page - 1) * this.pageSize, page * this.pageSize)
      }
    },
    toAddGoods () {
      this.$router.push('/add-goods')
    }
  },
  mounted () {
    this.getGoodsList(1)
  }
}
</script>

<style scoped lang="less">
.goods {
  margin: 20px;
}
.header {
  display: flex;
  button {
    margin-left: 20px;
  }
}
.wrapper {
  margin-top: 20px;
}
</style>
