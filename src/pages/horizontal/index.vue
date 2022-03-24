<template>
  <view class="container"> 
    <view class="list"  >
      <view class="list-item" v-for="(item,index) in list" :key="index" >
        <image mode="widthFix" :src='item.url' />  
      </view>
    </view >
  </view>
</template>

<script>
export default {
  data(){
    return{
      list:[],
      count:1
    }
  },
  methods:{
    getList(){
        let currentIndex = 0;
        for(let i = this.count;i<30+this.count;i++){
            this.list.push({
              url:`https://cdn.jsdelivr.net/gh/520ck/cdnseovx/1/I/l/1/I/l/seovx-com-2018%20(${i}).jpg`
            })
            currentIndex = i;
        }
        this.count = currentIndex
        uni.showToast({title:'图片加载中',icon:'none'})
    }
  },
  onLoad(){
    this.getList()
  },
  onReachBottom(){
        this.getList()
    }
}
</script>

<style lang='scss' scoped>
  .list{
    display: flex;
    flex-wrap: wrap;
    .list-item{
      width:50%;
      border: 5rpx solid #fff;
    }
  }
</style>