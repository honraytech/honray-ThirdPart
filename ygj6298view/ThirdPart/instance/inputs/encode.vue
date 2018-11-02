<template>
  <div>
    <el-input
      v-model="original"
      placeholder="请输入需要加密的文本"
      clearable=""
      @change="inputChange"></el-input>
    <el-input
      v-model="encrypted"
      placeholder="显示密文"
      disabled></el-input>
  </div>
</template>

<style>
</style>

<script>
  export default {
    props: {
      fieldId: {
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
      name: {
        type: String,
        default() {
          return ''
        }
      },
      title: {
        type: String,
        default() {
          return ''
        }
      },
      rule: {
        type: Array,
        default() {
          return []
        }
      },
      disabled: {
        type: Boolean,
        default() {
          return false
        }
      },
      config: {
        type: Object,
        default() {
          return {}
        }
      },
      value: {
        default() {
          return ''
        }
      }
    },
    data() {
      return {
        original: '',
        encrypted: ''
      }
    },
    created() {
      console.log('fieldId = ', this.fieldId)
      console.log('dataId = ', this.dataId)
      console.log('name = ', this.name)
      console.log('title = ', this.title)
      console.log('rule = ', this.rule)
      console.log('disabled = ', this.disabled)
      console.log('config = ', this.config)
      console.log('value = ', this.value)
      if (!_g.isRealEmpty(this.value)) {
        this.original = _.cloneDeep(this.value)
      }
    },
    methods: {
      inputChange(val) {
        this.$emit('input', this.original)
      }
    }
  }
</script>