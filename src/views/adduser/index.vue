<template>
  <div class="app-container">
    <el-form ref="form" :model="user" label-width="120px">
      <el-form-item label="用户名称">
        <el-input v-model="user.username"/>
      </el-form-item>
      <el-form-item label="设置密码">
        <el-input v-model="user.password"/>
      </el-form-item>
      <el-form-item label="年龄">
        <el-input v-model="user.age"/>
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="user.email"/>
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
        user: {
          'username': '',
          'password': 0,
          'age': 0,
          'email': ''
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
          url: 'http://localhost:8090/user/add',
          // 解决 post 请求无法携带数据问题
          transformRequest: [function(data) {
            return JSON.stringify(data)
          }],
          data: vm.user

        }).then(function(resp) {
          vm.$router.push('/users')
        })
      },
      onCancel() {
        var vm = this
        vm.$router.push('/users')
      }
    }
  }
</script>

<style scoped>
  .line {
    text-align: center;
  }
</style>

