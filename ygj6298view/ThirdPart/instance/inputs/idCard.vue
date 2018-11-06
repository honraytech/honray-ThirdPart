<template>
  <div>
    <el-input
      v-model="idCard"
      placeholder="请输入身份证号"
      clearable></el-input>
  </div>
</template>

<style>
</style>

<script>
  import base64 from 'base-64'
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
        idCard: ''
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
        this.idCard = _.cloneDeep(this.value)
      }
      this.rule.push({ validator: this.validator, trigger: 'blur' })
    },
    methods: {
      validator(rules, value, callback) {
        this.$emit('input', this.idCard)
        let result = this.checkId(this.idCard)
        if (!result.isPass) {
          callback(new Error(result.value))
        }
        let info = this.getInfo(result.value)
        let obj = {}
        let sexField = base64.decode(this.config.sex).split('-')[1]
        let birthField = base64.decode(this.config.birth).split('-')[1]
        obj[sexField] = info.gender === '1' ? '男' : '女'
        obj[birthField] = moment(info.birth).unix()
        this.$emit('setFormData', obj)
        callback()
      },
      // 检查身份证号码
      checkId(pId) {
        let result = { isPass: true, value: pId }
        let arrVerifyCode = [1, 0, 'x', 9, 8, 7, 6, 5, 4, 3, 2]
        let Wi = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]
        let Checker = [1, 9, 8, 7, 6, 5, 4, 3, 2, 1, 1]
        if (pId.length !== 15 && pId.length !== 18) {
          // return "身份证号共有15位或18位"
          result.isPass = false
          result.value = '身份证号共有15位或18位'
          return result
        }
        let Ai = pId.length === 18 ? pId.substring(0, 17) : pId.slice(0, 6) + '19' + pId.slice(6, 16)
        if (!/^\d+$/.test(Ai)) {
          // return "身份证除最后一位外，必须为数字！"
          result.isPass = false
          result.value = '身份证除最后一位外，必须为数字！'
          return result
        }
        let yyyy = parseInt(Ai.slice(6, 10), 10)
        let mm = parseInt(Ai.slice(10, 12), 10) - 1
        let dd = parseInt(Ai.slice(12, 14), 10)
        let d = new Date(yyyy, mm, dd)
        let now = new Date()
        let year = d.getFullYear()
        let mon = d.getMonth()
        let day = d.getDate()
        if (year !== yyyy || mon !== mm || day !== dd || d > now || year < 1800) {
          // return "身份证输入错误！"
          result.isPass = false
          result.value = '身份证格式错误！'
          return result
        }
        let ret = 0
        for (let i = 0; i < 17; i++) {
          ret += Ai.charAt(i) * Wi[i]
        }
        Ai += arrVerifyCode[ret %= 11]
        if (pId.length === 18 && pId !== Ai) {
          result.isPass = false
          result.value = '身份证格式错误！'
          return result
        } else {
          result.value = Ai
          return result
        }
      },
      // 获取输入身份证号码
      getInfo() {
        let result = this.checkId(this.idCard)
        let value = result.value
        if (!result.isPass) {
          return
        }
        value = String(value)
        // 获取出生日期
        let birth = value.substring(6, 10) + '-' + value.substring(10, 12) + '-' + value.substring(12, 14)
        // 获取性别
        let gender = value.slice(14, 17) % 2 ? '1' : '2' // 1代表男性，2代表女性
        return { birth: birth, gender: gender }
      }
    }
  }
</script>