<template>
  <div class="index">
    <div class="search-wrapper">
      <h1>{{ msg }}</h1>
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
        <el-form-item>
          <el-select v-model="formInline.tag" placeholder="筛选条件">
            <el-option label="区域一" value="shanghai"></el-option>
            <el-option label="区域二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item style="width: 50%">
          <el-input style="width:100%" v-model="formInline.key" placeholder="搜索条件"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">查询</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="search-content">
      <div class="sc-item" v-for="(item, index) in jsonData" :key="index" @click="showContent(item, index)">
        <div class="sci-title">
          <span>{{item.name}}</span>
          <i class="el-icon-arrow-down" v-if="index === n"></i>
          <i class="el-icon-arrow-right" v-else></i>
        </div>
        <vue-json-pretty
          v-show="index === n"
          :path="'res'"
          :data=item
          @click="handleClick">
        </vue-json-pretty>
      </div>
    </div>
  </div>
</template>

<script>
import VueJsonPretty from 'vue-json-pretty'
import request from '@/service/request.js'

export default {
  components: {
    VueJsonPretty
  },
  data () {
    return {
      msg: 'Welcome to use show json parse',
      formInline: {
        tag: '',
        key: ''
      },
      n: 0,
      jsonData: [
        {
          id: 1,
          key: 'value',
          name: 'kira'
        },
        {
          id: 2,
          key: 'value',
          name: 'kira'
        }
      ]
    }
  },
  methods: {
    onSubmit () {
      console.log('submit!')
    },
    async getData () {
      try {
        const res = await request({
          url: '/api/addresses',
          method: 'post',
          data: this.formInline
        })
        this.jsonData = res.data.data.results
      } catch (error) {
        console.log(error)
      }
    },
    handleClick () {
      console.log('handleClick')
    },
    showContent (item, index) {
      this.n = index
    }
  }
}
</script>

<style scoped>
.search-wrapper{
  text-align: center;
}
.search-content{
  width: 80%;
  margin: 16px auto;
}
.sci-title{
  background: #d8e3e8;
  padding: 12px 16px;
  border-radius: 6px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: pointer;
  margin-bottom: 12px;
}

</style>
