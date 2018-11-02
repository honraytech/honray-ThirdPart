<template>
  <div>
    <el-button type="success" @click="add">添加</el-button>
    <el-table :data="tableData">
      <el-table-column
        v-for="(item, index) in column"
        :key="index"
        :prop="item.field_name"
        :label="item.show_name"></el-table-column>
      <el-table-column label="操作列">
        <template slot-scope="scope">
          <el-button type="primary" @click="edit(scope.row)">编辑</el-button>
          <el-button type="danger" @click="deleteRow(scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<style>
</style>

<script>
  export default {
    props: {
      dealId: {
        type: String,
        default() {
          return ''
        }
      },
      dataId: {
        type: String,
        default() {
          return ''
        }
      },
      row: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    data() {
      return {
        // 组件配置
        column: [],
        // 表格数据
        tableData: []
      }
    },
    created() {
      console.log('dealId = ', this.dealId)
      console.log('dataId = ', this.dataId)
      console.log('row = ', this.row)
      let data = {
        callback: this.questSelfMethod
      }
      let pos = {
        className: '',
        funcName: '',
        callback: this.getDealConfig
      }
      // 获取组件配置
      this.$emit('getDealConfig', pos)
      // 获取数据
      this.$emit('questSelfMethod', data)
    },
    methods: {
      add() {
        let data = {
          deal: {
            button_icon: '',
            button_style: '',
            button_txt: 1,
            dialog_height: null,
            dialog_title: '',
            dialog_width: 1200,
            id: 'cae9e4321030d472febf48743398a3ec',
            iden: '',
            is_query: 0,
            name: '添加',
            open_type: 20,
            pos: 20,
            show_type: 20,
            type: 20,
            url: '/content/form/4b4ee8a5f6ee54a11559e3a95be71f36/9316375f868ef336ccd57f6411b850fa'
          },
          data: [],
          callback: this.addCallback
        }
        this.$emit('dealEvent', data)
      },
      edit(row) {
        let data = {
          deal: {
            button_icon: '',
            button_style: '',
            button_txt: 1,
            dialog_height: null,
            dialog_title: '',
            dialog_width: 1200,
            id: 'a0b641447ac1083b79ab32205e32a77c',
            iden: '',
            is_query: 0,
            name: '编辑',
            open_type: 20,
            pos: 20,
            show_type: 30,
            type: 20,
            url: '/content/form/4b4ee8a5f6ee54a11559e3a95be71f36/9316375f868ef336ccd57f6411b850fa'
          },
          data: [row],
          callback: this.editCallback
        }
        this.$emit('dealEvent', data)
      },
      deleteRow(row) {
        let data = {
          deal: {
            button_icon: '',
            button_style: '',
            button_txt: 1,
            dialog_height: null,
            dialog_title: '',
            dialog_width: 1200,
            id: '18d42ccff177f5c56d4d3e39d46b7a17',
            iden: '',
            is_query: 0,
            name: '删除',
            open_type: 10,
            pos: 30,
            show_type: 50,
            type: 50,
            url: '/content/form/4b4ee8a5f6ee54a11559e3a95be71f36/9316375f868ef336ccd57f6411b850fa'
          },
          data: [row],
          callback: this.deleteCallback
        }
        this.$emit('dealEvent', data)
      },
      // 添加回调
      addCallback(res) {
        console.log(res)
        _g.reloadPage(this)
      },
      // 编辑回调
      editCallback(res) {
        console.log(res)
        _g.reloadPage(this)
      },
      // 删除回调
      deleteCallback(res) {
        console.log(res)
        _g.reloadPage(this)
      },
      // 获取数据回调
      questSelfMethod(res) {
        console.log(res)
        this.tableData = res.data.list
      },
      // 获取组件配置回调
      getDealConfig(res) {
        console.log(res)
        this.column = res.data.column
      }
    }
  }
</script>