<template>
  <div class="dashboard-container">
    <el-form ref="form" :model="form" label-width="80px">
      <el-form-item label="学校名称">
        <el-input v-model="form.name" />
      </el-form-item>
      <el-form-item label="位置">
        <el-input v-model="form.where" />
      </el-form-item>
      <el-form-item label="类型">
        <el-input v-model="form.leixing" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">立即创建</el-button>
        <el-button>取消</el-button>
      </el-form-item>

    </el-form>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'School',
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  data() {
    return {
      apiModel: 'school',
      form: {}
    }
  },
  mounted() {
    if (this.$route.query._id) {
      this.$http.post('/api/' + this.apiModel + '/get', { _id: this.$route.query._id }).then(res => {
        if (res && res.length > 0) {
          this.form = res[0]
        }
      })
    }
  },
  methods: {
    onSubmit() {
      console.log('222:', 222)
      if (this.form._id) {
        this.$http.post(`/api/${this.apiModel}/update`, this.form).then(res => {
          console.log('bar:', res)
          this.$router.push({ path: this.apiModel })
          this.form = {}
        })
      } else {
        this.$http.post('/api/' + this.apiModel + '/add', this.form).then(res => {
          console.log('bar:', res)
          this.$router.push({ path: this.apiModel })
          this.form = {}
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .dashboard {
    &-container {
      margin: 30px;
    }
    &-text {
      font-size: 30px;
      line-height: 46px;
    }
  }
</style>
