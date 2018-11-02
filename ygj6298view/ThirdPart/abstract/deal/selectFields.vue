<template>
  <div>
    <el-select 
      v-model="selectData"
      multiple
      clearable
      @change="selectChange">
      <el-option
        v-for="(item, index) in fields"
        :key="index"
        :label="item.title"
        :value="item.field_id"></el-option>
    </el-select>
  </div>
</template>

<style>
</style>

<script>
  export default {
    props: {
      // 配置值字段名
      name: {
        type: String,
        default() {
          return ''
        }
      },
      // 配置值字段标题
      title: {
        type: String,
        default() {
          return ''
        }
      },
      // 当前页面模型id
      contentId: {
        type: String,
        default() {
          return ''
        }
      },
      // 当前页面模型所关联的表单模型id
      modelId: {
        type: String,
        default() {
          return ''
        }
      },
      value: {
        default() {
          return []
        }
      }
    },
    data() {
      return {
        // 表格字段
        fields: [],
        selectData: []
      }
    },
    created() {
      console.log('contentId = ', this.contentId)
      console.log('name = ', this.name)
      console.log('title = ', this.title)
      console.log('modelId = ', this.modelId)
      console.log('value = ', this.value)
      if (!_g.isRealEmpty(this.value)) {
        _(this.value).forEach((item) => {
          this.selectData.push(item.field_id)
        })
      }
      let data = {
        id: this.modelId,
        callback: this.getFieldsCallback
      }
      this.$emit('getFields', data)
    },
    methods: {
      // 获取表单结构回调
      getFieldsCallback(res) {
        this.fields = res.data
        console.log(res)
      },
      selectChange(val) {
        let data = []
        _(this.selectData).forEach((item) => {
          let obj = {
            field_id: item,
            show_name: _.find(this.fields, { 'field_id': item }).title
          }
          data.push(obj)
        })
        this.$emit('input', data)
      }
    }
  }
</script>