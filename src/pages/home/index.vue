<template>
  <view class="container">
    <view class="tab">
      <view class="tab-title">
        <uni-segmented-control :current="current" :values="items" @clickItem="onClickItem" styleType="text" activeColor="#dd5096"></uni-segmented-control>
      </view>
      <view class="tab-search iconfont iconsearch"></view>
    </view>
        <view class="content">
            <view v-if="current === 0">
                <home-recommend/>
            </view>
            <view v-if="current === 1">
                <home-category/>
            </view>
            <view v-if="current === 2">
                <home-new/>
            </view>
            <view v-if="current === 3">
                <home-album/>
            </view>
        </view>
  </view>
</template>

<script>
import { uniSegmentedControl} from "@dcloudio/uni-ui";
import homeRecommend from './components/home-recommend';
import homeNew from './components/home-new';
import homeCategory from './components/home-category';
import homeAlbum from './components/home-album';
export default {
  data() {
    return {
      current: 1,
      items: ["推荐", "分类","最新", "专辑"],
    };
  },
  components: {
    uniSegmentedControl,
    homeRecommend,
    homeNew,
    homeCategory,
    homeAlbum,
  },
  methods: {
    onClickItem(e) {
      if (this.current != e.currentIndex) {
        this.current = e.currentIndex;
      }
    },
  },
  onLoad() {
    // uni.request({
    //   url: "http://service.picasso.adesk.com/v3/homepage/vertical",
    //   success(data) {
    //     console.log(data);
    //   },
    // });
  },
};
</script>

<style scoped type="text/scss">
      .tab{
         position: relative;
      }
      .tab-title{
        width: 60%;
        margin:0 auto;
      }
      .tab-search{
        position: absolute;
        top:50%;
        right: 5%;
        transform:translateY(-50%)
      }
    
</style>