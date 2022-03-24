<template>
  <scroll-view scroll-y class="scroll-container"  @scrolltolower="handleToLower">
      <view class="swiper">
        <swiper autoplay indicator-dots circular>
          <swiper-item v-for="item in banner" :key="item.id">
            <image nodel="heightFix" :src="item.thumb"  />
            {{item.thumb}}
          </swiper-item>
        </swiper>
      </view>
      <view class="album-list">
        <navigator class="album-item" v-for="item in album" :key="item.id" :url="`/pages/home/components/home-album-info?id=${item.id}`" >
          <view class="picture">
            <image model="aspectFill" :src="item.cover" />
          </view>
          <view class="info">
            <view class="title">{{item.name}}</view>
            <view class="desc">{{item.desc}}</view>
            <view class="attention">+ 关注</view>
          </view>
        </navigator>
      </view>
  </scroll-view>
</template>

<script>
export default {
  data(){
    return {
      params:{
        limit:30,
        order:'new',
        skip:0
      },
      banner:[],
      album:[],
      hasmore:true
    }
  },
  methods:{
    getList(){
      this.request({
        url: 'http://service.picasso.adesk.com/v1/wallpaper/album',
        data:this.params
      }).then(res=>{
        if(res.data.res.album.length==0){
          this.hasmore = false;
          return;
        }
        if(this.banner.length == 0)
        this.banner = res.data.res.banner;
        
        
        this.album = [...this.album,...res.data.res.album];
        
      })
    },
    handleToLower(){
      if(this.hasmore){
        this.params.skip+=this.params.limit
        this.getList()
      }else{
        uni.showToast({
          title:`给不了更多了(〃'▽'〃)`,
          icon:'none'
        })
      }
    }
  },
  mounted(){
    uni.setNavigationBarTitle({title:'专辑'})
    this.getList()
  }
}
</script>

<style lang="scss" scoped>
.scroll-container{
  height:calc(100vh - 42px)
}
  .swiper{
    swiper{
      height: calc(752rpx / 2.3);
      image{
        height: 100%;
      }
    }
  }
  .album-list{
    padding: 10rpx;
    .album-item{
      display: flex;
      padding: 20rpx;
      border-bottom: 2rpx solid #9bb0cd;
      margin: 10rpx 0;
      .picture{
        flex:1;
        image{
          width: 200rpx;
          height:200rpx;
        }
      }
      .info{
        overflow: hidden;
        flex:2;
        .title{
          font-size: 36rpx;
          font-weight: 600;
        }
        .desc{
          font-size:24rpx;
          padding: 10rpx 0;
          text-overflow: ellipsis;
          overflow: hidden;
          white-space: nowrap;
        }
        .attention{
          width: 112rpx;
          padding: 10rpx;
          float: right;
          border: 2rpx solid $color;
          color: $color;
        }
      }
    }
  }
</style>