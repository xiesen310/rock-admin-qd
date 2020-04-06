<template>
  <div class="app-container">
    <div align="right">
      <el-button type="success" @click="addDataSet">新建数据集</el-button>
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

      <el-table-column label="数据集ID">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>

      <el-table-column label="数据集名称">
        <template slot-scope="scope">
          {{ scope.row.datasetName }}
        </template>
      </el-table-column>

      <el-table-column label="主题">
        <template slot-scope="scope">
          {{ scope.row.topic }}
        </template>
      </el-table-column>
      <el-table-column label="分区数">
        <template slot-scope="scope">
          {{ scope.row.partitionNum }}
        </template>
      </el-table-column>
      <el-table-column label="副本数">
        <template slot-scope="scope">
          {{ scope.row.replicationNum }}
        </template>
      </el-table-column>
      <el-table-column label="约束">
        <template slot-scope="scope">
          {{ scope.row.jsonSchema }}
        </template>
      </el-table-column>
      <el-table-column label="创建时间">
        <template slot-scope="scope">
          {{ scope.row.createTime }}
        </template>
      </el-table-column>
      <el-table-column label="描述">
        <template slot-scope="scope">
          {{ scope.row.remark }}
        </template>
      </el-table-column>


      <el-table-column label="操作" align="center" width="230" class-name="small-padding fixed-width">
        <template slot-scope="scope">
          <el-button type="primary" size="mini" @click="editProduct(scope.row.id)">
            编辑
          </el-button>
          <el-button size="mini" type="danger" @click="delDataset(scope.row.id)">
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
          url: 'http://localhost:8090/dataset?pageNum=' + vm.listQuery.page + '&pageSize=' + vm.listQuery.limit
        }).then(function(resp) {
          console.log(resp)
          vm.total = resp.data.data.total
          vm.list = resp.data.data.list
        })
      },

      fetchData() {
        var vm = this
        this.axios({
          method: 'get',
          url: 'http://localhost:8090/dataset/list'
        }).then(function(resp) {
          vm.list = resp.data
        })
      },
      addDataSet() {
        this.$router.push('/adddataset/index')
      },
      editProduct(id) {
        this.$router.push('/editdataset/index/' + id)
      },
      delDataset(id) {
        var vm = this
        this.axios({
          method: 'get',
          url: 'http://localhost:8090/dataset/delete/' + id
        }).then(function(resp) {
          console.log(resp)
          if (resp.data.code == 0) {
            vm.$message({
              message: '删除成功',
              type: 'success'
            })
            // vm.fetchData()
            vm.getList()
          }
        }).catch(function(error) {
          vm.$message.error('删除失败')
        })
      }
    }
  }
</script>
