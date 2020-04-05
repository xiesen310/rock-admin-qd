<template>
  <div class="app-container">
    <div align="right">
      <el-button type="success" @click="adduser">添加用户</el-button>
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

      <el-table-column label="用户ID">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>

      <el-table-column label="用户名">
        <template slot-scope="scope">
          {{ scope.row.username }}
        </template>
      </el-table-column>

      <el-table-column label="年龄">
        <template slot-scope="scope">
          {{ scope.row.age }}
        </template>
      </el-table-column>
      <el-table-column label="邮箱">
        <template slot-scope="scope">
          {{ scope.row.email }}
        </template>
      </el-table-column>
      <el-table-column label="创建时间">
        <template slot-scope="scope">
          {{ scope.row.salePrice }}
        </template>
      </el-table-column>

      <el-table-column label="Actions" align="center" width="230" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button type="primary" size="mini" @click="editUser(scope.row.id)">
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
      this.getList()
    },
    methods: {
      getList() {
        // 得到一个 pageInfo 对象
        var vm = this
        this.axios({
          method: 'get',
          url: 'http://localhost:8090/user?pageNum=' + vm.listQuery.page + '&pageSize=' + vm.listQuery.limit
        }).then(function(resp) {
          vm.total = resp.data.data.total
          vm.list = resp.data.data.list
        })

      },

      adduser() {
        this.$router.push('/adduser/index')
      },

      editUser(id) {
        console.log(id)
        this.$router.push('/edituser/index/' + id)
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
