<template >
  <view @touchstart="handleTouchStart" @touchend="handleTouchEnd">
    <view class="userInfo">
      <view class="icon">
        <image
          mode="widthFix"
          :src="imgDetail.user.avatar"
          v-if="imgDetail.user"
        />
      </view>
      <view class="name">
        <view>{{ imgDetail.user.name }}</view>
        <view class="time">{{ imgDetail.cnTime }}</view>
      </view>
    </view>
    <view class="img">
      <image :src="imgDetail.picture" mode="widthFix" />
    </view>
    <view class="start">
      <view class="startNumber">
        <text class="iconfont icondianzan">{{ " " + imgDetail.rank }}</text>
      </view>
      <view class="love">
        <text class="iconfont iconshoucang"> {{ " " + "收藏" }}</text>
      </view>
    </view>
    <comment :list="hot" v-if="hot.length > 1">
      <view class="iconfont iconhot1">最热评论</view>
    </comment>
    <comment :list="comment" v-if="comment.length > 1">
      <view class="iconfont iconpinglun">最新评论</view>
    </comment>

    <view class="download">
      <view class="download-btn" @click="handleDownload"> 下载图片 </view>
    </view>
  </view>
</template>
<script>
import moment from "moment";
import comment from "@/components/comment";
export default {
  components: { comment },
  data() {
    return {
      imgDetail: {},
      comment: [],
      hot: [],
      start: 0,
      end: 0,
    };
  },
  methods: {
    getComment() {
      this.request({
        url: `https://service.picasso.adesk.com/v2/vertical/vertical/${this.imgDetail.id}/comment`,
      }).then((res) => {
        this.comment = res.data.res.comment.map((e) => {
          return { ...e, atime: moment(e.atime * 1000).fromNow() };
        });
        this.hot = res.data.res.hot.map((e) => {
          return { ...e, atime: moment(e.atime * 1000).fromNow() };
        });
        //console.log(this.comment)
      });
    },
    init(count) {
      //初始化页面信息
      let { list, index } = getApp().globalData;
      let changeIndex = index + count;
      if (changeIndex < 0 || changeIndex > list.length - 1) {
        uni.showToast({ title: "到底了~", icon: "none" });
        return;
      }
      getApp().globalData.index = changeIndex;
      //console.log(getApp().globalData.index)
      this.imgDetail = list[changeIndex];
      this.imgDetail.picture = this.imgDetail.thumb;
      this.imgDetail.cnTime = moment(this.imgDetail.atime * 1000).fromNow();
      this.getComment();
    },
    //滑动开始
    handleTouchStart(event) {
      this.start = event.changedTouches[0].clientX;
    },
    //滑动结束
    handleTouchEnd(event) {
      this.end = event.changedTouches[0].clientX;
      if (this.start - this.end > 100) {
        this.init(1);
      }
      if (this.end - this.start > 100) {
        this.init(-1);
      }
    },
    async handleDownload() {
        console.log(this.imgDetail)
      const result = await uni.downloadFile({ url: this.imgDetail.img });
      console.log(result)
    //   const msg = await uni.saveImageToPhotosAlbum({ filePath: result[1] });
    //   console.log(msg)
    //   uni.showToast({ title: msg.errMsg, icon: "none" });
    },
  },
  onLoad() {
    //页面加载钩子
    this.init(0);
  },
};
</script>

<style lang="scss" scoped>
.userInfo {
  display: flex;
  padding: 20rpx;
  .icon {
    padding: 0 20rpx;
    image {
      width: 88rpx;
      border-radius: 50%;
    }
  }
  .name {
    font-weight: 600;
    .time {
      color: #ccc;
      font-size: 24rpx;
    }
  }
}
.img {
  border: 5rpx solid white;
}
.start {
  display: flex;
  height: 80rpx;
  .startNumber {
    display: flex;
    flex: 1;
    align-items: center;
    justify-content: center;
  }
  .love {
    display: flex;
    flex: 1;
    align-items: center;
    justify-content: center;
  }
}
.iconhot1 {
  color: red;
  font-size: 36rpx;
  padding: 20rpx;
}
.iconpinglun {
  color: blue;
  font-size: 36rpx;
  padding: 20rpx;
}
.download {
  height: 93rpx;
  display: flex;
  align-content: center;
  justify-content: center;
  .download-btn {
    width: 95%;
    height: 72%;
    background-color: $color;
    color: #fff;
    font-size: 40rpx;
    font-weight: 600;
    display: flex;
    align-content: center;
    justify-content: center;
  }
}
</style>