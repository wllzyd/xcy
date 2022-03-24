<template>
  <view>
    <view class="list">
      <view class="list-item" v-for="item in category" :key="item.id" @click="handleClick(item.id)" >
        <image mode="aspectFill" :src='item.cover'/>
        <view class="title">{{item.name}}</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  mounted(){
    uni.setNavigationBarTitle({title:'分类'})
    this.init()
  },
  data(){
    return{
      category:[]
    }
  },
  methods:{
    init(){
      this.request({
        url: 'http://service.picasso.adesk.com/v1/vertical/category',
      }).then(res=>{
          this.category = res.data.res.category
      })
    },
    handleClick(id){
      
      uni.navigateTo({url:`/pages/home/components/home-category-info?id=${id}`})
    }
  }
}
</script>

<style lang="scss" scoped>
  .list{
    display: flex;
    flex-wrap: wrap;
    .list-item{
      width: 33.33%;
      position: relative;
      border: 5rpx solid #fff;
      image{
        height:240rpx
      }
      .title{
        position:absolute;
        width: 100%;
        height: 50rpx;
        left: 0;
        bottom: 0;
        color: #ffffff;
        //css3 渐变
        background: linear-gradient(to right top,rgba(0,0,0,.2),rgba(0,0,0,0));
        font-size: 40rpx;
        display: flex;
        align-items: center;
        padding-left: 20rpx;
      }
    }
  }

</style>