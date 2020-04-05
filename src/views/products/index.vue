<template>
  <div class="app-container">
    <div align="right">
      <el-button type="success" @click="addproduct">添加商品</el-button>
    </div>

    <el-table
      v-loading="listLoading"
      :data="list"
      border
      fit
      highlight-current-row
    >
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index + 1 }}
        </template>
      </el-table-column>

      <el-table-column label="商品ID">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>

      <el-table-column label="商品名称">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>

      <el-table-column label="商品图片">
        <template slot-scope="scope">
          {{ scope.row.images }}
        </template>
      </el-table-column>
      <el-table-column label="商品价格">
        <template slot-scope="scope">
          {{ scope.row.price }}
        </template>
      </el-table-column>
      <el-table-column label="实际价格">
        <template slot-scope="scope">
          {{ scope.row.salePrice }}
        </template>
      </el-table-column>
      <el-table-column label="商品卖点">
        <template slot-scope="scope">
          {{ scope.row.salePoint }}
        </template>
      </el-table-column>


      <el-table-column label="Actions" align="center" width="230" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button type="primary" size="mini" @click="editProduct(scope.row.id)">
            编辑
          </el-button>
          <el-button size="mini" type="danger" @click="delProduct(scope.row.id)">
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit"
                @pagination="getList"/>

  </div>
</template>

<script>
  import Pagination from '@/components/Pagination'

  export default {
    components: { Pagination },
    filters: {
      statusFilter(status) {
        const statusMap = {
          published: 'success',
          draft: 'gray',
          deleted: 'danger'
        }
        return statusMap[status]
      }
    },
    data() {
      return {
        list: null,
        total: 0,
        listQuery: {
          page: 1,
          limit: 10
        }
      }
    },
    created() {
      // this.fetchData()
      this.getList()
    },
    methods: {
      getList() {
        // 得到一个 pageInfo 对象
        var vm = this
        this.axios({
          method: 'get',
          url: 'http://localhost:8090/product/pageInfo?pageNum=' + vm.listQuery.page + '&pageSize=' + vm.listQuery.limit
        }).then(function(resp) {
          vm.total = resp.data.total
          vm.list = resp.data.list
        })

      },

      fetchData() {
        var vm = this
        this.axios({
          method: 'get',
          url: 'http://localhost:8090/product/list'
        }).then(function(resp) {
          vm.list = resp.data
        })
      },
      addproduct() {
        this.$router.push('/addproduct/index')
      },
      editProduct(id) {
        this.$router.push('/editproduct/index/' + id)
      },
      delProduct(id) {
        var vm = this
        this.axios({
          method: 'get',
          url: 'http://localhost:8090/product/delete/' + id
        }).then(function(resp) {
          if (resp.data == 'success') {
            vm.$message({
              message: '删除成功',
              type: 'success'
            })
            vm.fetchData()
          }
        }).catch(function(error) {
          vm.$message.error('删除失败')
        })
      }
    }
  }
</script>
