TODO 点击员工管理，自动跳转到分页查询，这个界面要借鉴黑马苍穹外卖的UI设计，有分页查询（包括关键字查询），新增、编辑、删除员工，其中删除员工的后端config代理我没配置，这个得在按钮的@click的方法里配置
TODO
新增员工跳转到src\views\business\employeefunction\AddEmployeeView.vue界面，编辑员工跳转到src\views\business\employeefunction\EditEmployeeView.vue，这个你们自己添加跳转按钮和配置路由
TODO 要注意编辑员工和删除员工的按钮是一个员工一个，新增员工只有一个
TODO 请求时候data里面的数据得根据前端设计设置，这个你们来加
<template>
  <div>
    <h1>商家端管理员工</h1>
    <!-- 这里可以添加一个搜索框，用于关键字查询 -->
    <el-input placeholder="请输入员工姓名" v-model="searchKey" class="search-input" @keyup.enter.native="searchEmployee">
      <el-button slot="append" icon="el-icon-search" @click="searchEmployee"></el-button>
    </el-input>

    <!-- 新增员工按钮 -->
    <el-button type="primary" @click="addEmployee">新增员工</el-button>

    <!-- 员工列表 -->
    <el-table :data="records" style="width: 100%">
      <el-table-column prop="id" label="ID" width="180"></el-table-column>
      <el-table-column prop="name" label="姓名"></el-table-column>
      <el-table-column prop="sex" label="性别">
        <template slot-scope="scope">
          <span v-if="scope.row.status === '0'">男</span>
          <span v-else-if="scope.row.status === '1'">女</span>
        </template>
      </el-table-column>
      <el-table-column prop="phone" label="电话号码"></el-table-column>
      <el-table-column prop="account" label="账号"></el-table-column>
      <el-table-column prop="permission" label="权限">
        <template slot-scope="scope">
          <span v-if="scope.row.status === '0'">超级管理员</span>
          <span v-else-if="scope.row.status === '1'">普通管理员</span>
        </template>
      </el-table-column>
      
      <el-table-column prop="createTime" label="注册时间"></el-table-column>
      <el-table-column prop="updateTime" label="最后登录时间"></el-table-column>
      <!-- 操作列 -->
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button type="text" @click="editEmployee(scope.row)">编辑</el-button>
          <el-button type="text" @click="deleteEmployee(scope.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
    <el-pagination :current-page="currentPage" :page-size="pageSize" layout="prev, pager, next" :total="total" @current-change="fetchEmployees">
    </el-pagination>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    data () {
      return {
        total: null,
        records: [],
        currentPage:1,//当前页码
        pageSize:10,//每页显示的条目数
        form:{
          id:null,
          name:'',
          sex:'',
          phone:'',
          account:'',
          permission:'',
          createTime:'',
          updateTime:''
        }
      };
    },
    methods: {

    },
    mounted () {
      axios({//员工分页查询
        url: '/api/business/employee/page',
        method: 'get',
        headers: {
          "token": localStorage.getItem('token')
        },
        data: {
          name: null,//就是输入框得到的要查询员工的名字，默认没有
          page: this.currentPage,
          pageSize: null,
        }
      }).then(res => {
        this.total = res.data.data.total
        this.records = res.data.data.records
      }).catch(err => {
        console.log(err.data.msg)
      })
    }
  }
</script>

<style>

</style>