<template>
  <div class="app-container">
    <el-form ref="form" :model="dataset" label-width="120px">
      <el-form-item label="数据集名称">
        <el-input v-model="dataset.datasetName"/>
      </el-form-item>
      <el-form-item label="主题">
        <el-input v-model="dataset.topic"/>
      </el-form-item>
      <el-form-item label="分区数">
        <el-input v-model="dataset.partitionNum"/>
      </el-form-item>
      <el-form-item label="副本数">
        <el-input v-model="dataset.replicationNum"/>
      </el-form-item>
      <el-form-item label="约束">
        <el-input v-model="dataset.jsonSchema"/>
      </el-form-item>
      <el-form-item label="备注">
        <el-input v-model="dataset.remark"/>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="onSubmit">添加</el-button>
        <el-button @click="onCancel">取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  // 发送 post 请求携带数据时需要导入该模块
  import Qs from 'qs'

  export default {
    data() {
      return {
        dataset: {
          'datasetName': '',
          'topic': '',
          'partitionNum': 1,
          'replicationNum': 1,
          'jsonSchema': '',
          'remark': ''
        }
      }
    },
    methods: {
      onSubmit() {
        var vm = this
        this.axios({
          headers: {
            'Content-Type': 'application/json'
          },
          method: 'post',
          url: 'http://localhost:8090/dataset/add',
          // 解决 post 请求无法携带数据问题
          transformRequest: [function(data) {
            return JSON.stringify(data)
          }],
          data: vm.dataset

        }).then(function(resp) {
          vm.$router.push('/dataset')
        })
      },
      onCancel() {
        var vm = this
        vm.$router.push('/dataset')
      }
    }
  }
</script>

<style scoped>
  .line {
    text-align: center;
  }
</style>

