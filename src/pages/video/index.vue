<template>
  <view class="container"> 
    <view class="list" >
      <view class="list-item" v-for="item in list" :key="item.vid" >
        <image :src="item.cover" @click="handleClick($event,item)"/>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data(){
    return{
      list:[]
    }
  },
  methods:{
    getList(){
      this.request({
        url: 'https://api.isoyu.com/api/Video/video_type?type=2',
      }).then(res=>{
        this.list = res.data.data;
        console.log(this.list)
      })
    },
    handleClick(_,item){
      console.log(item)
      getApp().globalData.vodeoInfo = item
      uni.navigateTo({url:'/pages/videoPlay/index'})
      console.log(1)
    }
  },
  onLoad(){
    this.getList();
  }
}
</script>

<style lang='scss' scoped>
  .list{
    display: flex;
    flex-wrap: wrap;
    .list-item{
      width: 100%;
      border:5rpx solid #fff;
    }
    
  }
</style>