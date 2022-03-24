<template>
  <view class="container">
      <image :src="videoObj.cover"/>
    <view class="tool">
      <view :class=" ['iconfont',muted?'iconjingyin':'iconshengyin'] " @click="changeSound"></view>
      <view class=" iconfont iconzhuanfa">
          <button open-type="share" ></button>
      </view>
    </view>
    <view class="video">
         <video :muted="muted" :src="videoObj.mp4_url" objectFit="fill"/>
    </view>
   
  </view>
</template>

<script>
export default {
    data(){
        return {
            videoObj:{},
            muted:false
        }
    },
    onLoad(){
       this.videoObj = getApp().globalData.vodeoInfo
       console.log(this.videoObj)
    },
    methods:{
        changeSound(){
            this.muted = !this.muted
        }
    }
}
</script>

<style lang="scss" scoped>
    .container{
        position: relative;
        image{
            position:absolute;
            width: 100vw;
            height: 100vh;
            z-index: -1;
            filter:blur(20px)
        }
        .tool{
            height: 80rpx;
            display: flex;
            justify-content: flex-end;
            .iconfont{
                width: 80rpx;
                background-color: rgba(0,0,0,.2);
                color:#fff;
                font-size:50rpx;
                border-radius: 40rpx;
                display: flex;
                justify-content: center;
                align-items: center;
                margin-right: 20rpx;
            }
            .iconzhuanfa{
                position: relative;
                button{
                    position: absolute;
                    width: 100%;
                    height: 100%;
                    opacity: 0;
                }
            }
        }
        .video{
            display: flex;
            justify-content: center;
            
            video{
                width:360rpx;
            height:600rpx;
            }
        }
    }
</style>