TODO 点击订单管理，自动跳转到分页查询，这个界面要借鉴黑马苍穹外卖的UI设计，有分页查询（包括根据关键字号查询），修改订单状态，删除订单，订单分页查询详情
TODO 查看订单详情跳转到src\views\business\orderfunction\DetailOrderView.vue，这个你们自己添加响应按钮和配置路由，注意是一个订单一个“查看详情”按钮
TODO 请求的时候data里面的数据得根据前端设计填充
<template>
  <h1>商家端展示订单</h1>
</template>

<script>
  import axios from 'axios'
  export default {
    data () {
      return {
        total: '',
        records: []
      };
    },
    mounted () {
      axios({//订单分页查询
        url: '/api/business/order/page',
        headers: {
          "token": localStorage.getItem('token')
        },
        method: 'get',
        data: {
          status: '',
          page: '',
          pageSize: '',
          id: '',
          phone: '',//就是输入框得到的要查询订单对应的手机号码的名字，默认没有
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