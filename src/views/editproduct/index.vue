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
        <el-button type="primary" @click="onSubmit">提交</el-button>
        <el-button @click="onCancel">退出</el-button>
      </el-form-item>
    </el-form>

    <!--    <el-form ref="form" :model="form" label-width="120px">
          <el-form-item label="Activity name">
            <el-input v-model="form.name" />
          </el-form-item>
          <el-form-item label="Activity zone">
            <el-select v-model="form.region" placeholder="please select your zone">
              <el-option label="Zone one" value="shanghai" />
              <el-option label="Zone two" value="beijing" />
            </el-select>
          </el-form-item>
          <el-form-item label="Activity time">
            <el-col :span="11">
              <el-date-picker v-model="form.date1" type="date" placeholder="Pick a date" style="width: 100%;" />
            </el-col>
            <el-col :span="2" class="line">-</el-col>
            <el-col :span="11">
              <el-time-picker v-model="form.date2" type="fixed-time" placeholder="Pick a time" style="width: 100%;" />
            </el-col>
          </el-form-item>
          <el-form-item label="Instant delivery">
            <el-switch v-model="form.delivery" />
          </el-form-item>
          <el-form-item label="Activity type">
            <el-checkbox-group v-model="form.type">
              <el-checkbox label="Online activities" name="type" />
              <el-checkbox label="Promotion activities" name="type" />
              <el-checkbox label="Offline activities" name="type" />
              <el-checkbox label="Simple brand exposure" name="type" />
            </el-checkbox-group>
          </el-form-item>
          <el-form-item label="Resources">
            <el-radio-group v-model="form.resource">
              <el-radio label="Sponsor" />
              <el-radio label="Venue" />
            </el-radio-group>
          </el-form-item>
          <el-form-item label="Activity form">
            <el-input v-model="form.desc" type="textarea" />
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">Create</el-button>
            <el-button @click="onCancel">Cancel</el-button>
          </el-form-item>
        </el-form>-->
  </div>
</template>

<script>
  // 发送 post 请求携带数据时需要导入该模块
  import Qs from 'qs'

  export default {
    data() {
      return {
        product: null
      }
    }, created() {
      this.fetchDataById()
    },
    methods: {
      fetchDataById() {
        var id = this.$route.params.id
        var vm = this
        this.axios({
          method: 'get',
          url: 'http://localhost:8090/product/' + id
        }).then(function(resp) {
          vm.product = resp.data
          console.log(resp)
        })
      },

      onSubmit() {
        var vm = this
        this.axios({
          method: 'post',
          url: 'http://localhost:8090/product/update',
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

