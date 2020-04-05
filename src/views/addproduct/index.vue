<template>
  <div class="app-container">
    <el-form ref="form" :model="product" label-width="120px">
      <el-form-item label="商品名称">
        <el-input v-model="product.name"/>
      </el-form-item>
      <el-form-item label="商品价格">
        <el-input v-model="product.price"/>
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
        product: {
          'name': '',
          'price': 0
        }
      }
    },
    methods: {
      onSubmit() {
        var vm = this
        this.axios({
          method: 'post',
          url: 'http://localhost:8090/product/insert',
          // 解决 post 请求无法携带数据问题
          transformRequest: [function(data) {
            return Qs.stringify(data)
          }],
          data: vm.product

        }).then(function(resp) {
          vm.$router.push('/products')
        })
      },
      onCancel() {
        var vm = this
        vm.$router.push('/products')
      }
    }
  }
</script>

<style scoped>
  .line {
    text-align: center;
  }
</style>

