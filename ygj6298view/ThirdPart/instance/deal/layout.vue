<template>
  <div class="classes-wrap">
    <!-- 日历 -->
    <div class="calendar-box">
      <div class="select flex-align">
        <div class="btn" @click="selectDate('prev')"><i class="el-icon-arrow-left"></i></div>
        <div class="value">{{year}}年{{month}}月</div>
        <div class="btn" @click="selectDate('next')"><i class="el-icon-arrow-right"></i></div>
      </div>
      <div class="calendar flex">
        <div class="week" v-for="(item, index) in weekList" :key="index">{{item}}</div>
        <!-- 每月1号前空掉几天 -->
        <div class="date gray" v-for="(item, index) in day" :key="index"></div>
        <!-- 每月天数 -->
        <div class="date" :class="{ 'today': today ==  index}" v-for="(item,index) in dateList" :key="index">
          <div class="number">{{index}}</div>
          <div class="list">
            <div v-for="obj in item" :key="obj.id" class="online">
              <img :src="icon" alt="">
              <a>{{ obj[compConfig.className] }}</a>
            </div>
          </div>
        </div>
        <!-- 剩余空白格 -->
        <div class="date gray" v-for="(item, index) in blankNumber" :key="index"></div>
      </div>
    </div>
  </div>
</template>
<script>
import utf8 from 'utf8'
import base64 from 'base-64'
let icon = require('assets/images/icon_teacher.png')
export default {
  data() {
    return {
      year: '',            // 日历显示年份
      month: '',           // 日历显示月份
      weekList: ['日','一','二','三','四','五','六'],
      dateList: [],
      // 每月1号前空掉几天
      day: 0,
      // 天数
      dayNumber: 0,
      // 剩余空白格
      blankNumber: 0,
      today: 0,
      compConfig: {},
      icon: icon
    }
  },
  created() {
    let nowDate = new Date()
    this.year = nowDate.getFullYear()
    this.month = nowDate.getMonth() + 1
    this.today = nowDate.getDate()
    let postData = {
      className: '',
      funcName: '',
      callback: this.getDealConfig
    }
    this.getData()
  },
  methods: {
    // 获取数据回调
    questSelfMethod(res) {
      console.log(res)
      if (res.code === 200) {
        if (!_g.isRealEmpty(res.data.config)) {
          this.compConfig = res.data.config
        }
        if (!_g.isRealEmpty(res.data.result)) {
          this.dateList = res.data.result
          // 天数
          this.dayNumber = Object.keys(this.dateList).length
          if ((this.day + this.dayNumber) > 35) {
            this.blankNumber = 42 - this.day - this.dayNumber
          } else {
            this.blankNumber = 35 - this.day - this.dayNumber
          }
        }
      }
    },
    // 左右切换月份
    selectDate(type) {
      if (type === 'next') {
        if (this.month === 12) {
          this.month = 1
          this.year++
        } else {
          this.month++
        }
      } else {
        if (this.month === 1) {
          this.month = 12
          this.year--
        } else {
          this.month--
        }
      }
      this.getData()
    },
    // 获取时间
    async getData() {
      let nowDate = new Date(this.year + '-' + this.month + '-01')
      // 1号周几
      this.day = nowDate.getDay()
      // 本地调试
      // let obj = {
      //   path: 'http://a64api.wkuai.cc/',
      //   token: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.IjM3NmU1NWEzMmYzNTE4ZjFjN2E0M2IyMzRjMDMxZmQ0Ig.VX2k_8huD5tUBUezGZQwkyxaQC1KzEOyzuZXQkSoQmM',
      //   dealId: 'a9f1735cb64e8766769052063076ae16',
      //   params: {
      //     month: this.year + '-' + this.month
      //   }
      // }
      // let data = await _dev.debugCustomPage(this, obj)
      let data = {
        freeData: {
          month: this.year + '-' + this.month
        },
        callback: this.questSelfMethod
      }
      this.$emit('questSelfMethod', data)
    }
  }
}
</script>
<style scoped>
  .classes-wrap{
    position: relative;
    color: #333;
    font-weight: 400;
    background-color: #fff;
  }
  /* 日历 */
  .select{
    height: 36px;
    justify-content: center;
  }
  .select .btn{
    cursor: pointer;
  }
  .select .value{
    width: 300px;
    text-align: center;
    font-weight: bold;
  }
  .calendar{
    flex-wrap: wrap;
    border: 1px solid rgba(241,242,247,1);
    background-color: #fff;
  }
  .calendar>div{
    width: 14.285714%;
    height: 150px;
    border: 1px solid rgba(241,242,247,1);
    box-sizing: border-box;
  }
  .calendar .week{
    height: 36px;
    line-height: 36px;
    text-align: center;
    font-weight: bold;
    font-family:'MicrosoftYaHei-Bold';
  }
  .calendar .date{
    position: relative;
    cursor: pointer;
  }
  .calendar .date.today {
    background-color: #bdec9a;
  }
  .calendar .date.gray{
    background-color: gray;
    opacity: 0.1;
    cursor: not-allowed;
  }
  .calendar .date .add-btn{
    width: 22px;
    height: 22px;
    line-height: 22px;
    text-align: center;
    font-size: 18px;
    color: #fff;
    border-radius: 22px;
    background-color: #409EFF;
    position: absolute;
    top: 3px;
    right: 3px;
  }
  .calendar .date .number{
    width: 22px;
    height: 22px;
    margin: 3px;
    border: 1px solid #fff;
    text-align: center;
    line-height: 22px;
    border-radius: 22px;
    font-size: 10px;
    box-sizing: border-box;
  }
  .calendar .date .number.click{
    border: 1px solid #20A0FF;
    background-color: #20A0FF;
    color: #fff;
  }
  .calendar .date .list{
    height: 30px;
    line-height: 30px;
    box-sizing: border-box; 
    font-size: 14px;
    color: #fff;
  }
  .calendar .date .list a{
    font-size: 14px;
    color: #fff;
  }
  .calendar .date .list .online{
    background-color: #79C6FF;
    padding: 0 8px;
    display: flex;
    align-items: center;
    white-space: nowrap;
  }
  .calendar .date .list .offline{
    background-color: #A4DA89; 
    padding: 0 8px;
    display: flex;
    align-items: center;
    white-space: nowrap;
  }
  .calendar .date .list img{
    width: 20px;
    height: 20px;
    margin-right: 8px;
  }
</style>