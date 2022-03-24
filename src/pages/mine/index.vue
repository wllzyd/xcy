<template>
  <view class="container">
    <view class="list">
      <view class="item" v-for="(item,index) in count" :key="index">
        <image :src="`http://api.btstu.cn/sjbz/?lx=meizi?t=`+index" />
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data(){
    return {
      count:30
    }
  },
  onReachBottom(){
    this.count+=30
  }
}
</script>

<style>
  .item{
    border:5rpx solid #fff;
  }
</style>