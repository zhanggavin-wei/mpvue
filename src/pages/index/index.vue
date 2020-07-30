<template>
  <div class="home">
    <!-- 背景图片 -->
    <img :src="img.homeBg" alt="">
    <!-- 搜索框 -->
    <input type="text" placeholder="手镯"/>
    <!-- 筛选按钮组 -->
    <div class="home-filter">
      <div>
        <picker
          @change="cateChange"
          :value='curCate'
          range-key='label'
          :range='cates'>
          <span v-text="cates[curCate].label"></span>
          <img class="homeSan" :src="img.homeSan" alt="">
        </picker>
      </div>
      <div></div>
      <div></div>
      <div></div>
    </div>
    <!-- 商品列表 -->
    <div
      v-for="item in list"
      @tap='skipToDetail(item)'
      :key="item.id" class="home_good">
      <span v-text='item.name'></span>
      <span>-</span>
      <span v-text='item.price'></span>
    </div>
  </div>
</template>
<script>
import img from '@/utils/img'
import { goods } from '@/utils/data'

export default {
  data () {
    return {
      img,
      curCate: 0,
      cates: [
        { id: 1, label: '展会活动', value: 'a' },
        { id: 2, label: '母婴产品', value: 'b' },
        { id: 3, label: '女士服装', value: 'c' },
        { id: 4, label: '体育用品', value: 'd' }
      ],
      list: []
    }
  },
  onPullDownRefresh () {
    console.log('下拉刷新')
    this.list = goods
    setTimeout(() => {
      mpvue.stopPullDownRefresh()
    }, 1500)
  },
  onReachBottom () {
    // 分页
    this.list = [...this.list, ...goods]
  },
  onShow () {
    // 调接口
    this.list = goods
  },
  methods: {
    cateChange (e) {
      console.log(e.target.value)
      this.curCate = e.target.value
    }
  },
  skipToDetail (item) {
    mpvue.navigateTo({
      url: 'detail?id=' + item.id + '&name=' + item.name
    })
  },
  created () {
    // let app = getApp()
  }
}
</script>
<style scoped>
.home>img{
  width: 100%;
  height: 528rpx;
}
.home input:nth-of-type(1){
  box-sizing: border-box;
  margin-top: 20rpx;
  width: 700rpx;
  height: 80rpx;
  margin: 0 auto;
  background: url('../../assets/images/sosuo.png') no-repeat 5px center #eee;
  padding-left: 90rpx;
  border: 1rpx solid #eee;
  border-radius: 50px;
  font-size: 32rpx;
}
.home-filter {
  margin-top: 20rpx;
  padding: 0 20rpx;
  height: 80rpx;
  line-height: 80rpx;
  display: flex;
  border-top: 1px solid #ccc;
  border-bottom: 1px solid #ccc;
}
.home-filter>div {
  flex: 1;
  font-size: 24rpx;
}
.home_good {
  line-height: 350rpx;
  border-bottom: 1rpx solid orange;
}
.homeSan{
  width: 35rpx;
  height:35rpx;
  line-height: 80rpx;
}
</style>
