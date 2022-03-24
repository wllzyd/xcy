<template>
  <scroll-view scroll-y class="scroll-container"  @scrolltolower="handleToLower" v-if="months.DD"> 
      <!-- 推荐开始 -->
      <view class="recommend-container">
        <navigator class="recommend-item" v-for="item in recommend" :key="item.id" :url="`/pages/home/components/home-album-info?id=${item.target}`">
          <img mode='widthFix' :src="item.thumb" >
        </navigator>
      </view>
      <!-- 推荐结束 -->
      <!-- 月份日期文本开始 -->
      <view class="month-container" >
        <view class="month-info">
          <view class="month-date">
            <view class="day">{{months.DD}} / </view>
            <view class="month">{{months.MM}} 月</view>
          </view>
          <view class="month-text">{{months.title}}</view>
        </view>
        <view class="month-more">更多></view>
      </view>
      <!-- 月份日期文本结束 -->
      <!-- 月份图片开始 -->
      <view class="month-picture">
        <view class="month-picture-item" v-for="(item,index) in months.items" :key="item.id" >
          <to-img-detail :list="months.items" :index="index">
            <img mode="widthFix" :src="item.thumb+item.rule.replace('$<Height>',360)" >
          </to-img-detail>
        </view>
      </view>
      <!-- 月份图片结束 -->

      <view class="hot-container">
        <view class="hot-title">
          <text > 热门</text>
        </view>
        <view class="hot-picture">
          <view class="hot-picture-item" v-for="(item,index) in hots" :key="item.id" >
              <to-img-detail :list="hots" :index="index">
                <image mode="heightFill" :src="item.thumb">
              </to-img-detail>
          </view>
        </view>
      </view>
  </scroll-view>
</template>

<script>
import moment from 'moment';
import toImgDetail from '@/components/toImgDetail'
export default {
  components:{
    toImgDetail
  },
  data(){
    return {
    recommend:[],
    months:{
      MM:'',
      DD:''
    },
    hots:[],
    params:{
      limit:30,
      order:'hot',
      skip:0
    },
    hasmore:true
  }
  },
  methods:{
    getList(){
      console.log('开始请求','http://service.picasso.adesk.com/v3/homepage/vertical?limit=10&&order=hot&&skip=0')
      this.request({
      url: 'http://service.picasso.adesk.com/v3/homepage/vertical',
      data:this.params
    }).then(res=>{
      if(res.data.res.vertical.length==0){
        this.hasmore = false;
        return;
      }
      // 第一次初始化
      if(this.recommend.length==0){
        this.recommend = res.data.res.homepage[1].items
        this.months = res.data.res.homepage[3]
        this.months.MM = moment(this.months.stime).format("MM")
        this.months.DD = moment(this.months.stime).format("DD")
        this.months.title = this.months.title
      }
      
      this.hots = [...this.hots,...res.data.res.vertical]
      console.log('1231313')
    })
    
    },
    handleToLower(){
      if(this.hasmore){
        this.params.skip+=this.params.limit
        this.getList();
      }else{
        uni.showToast({
          title:`给不了更多了(〃'▽'〃)`,
          icon:'none'
        })
      }
      
    }
  },
  mounted(){
    this.getList()
    uni.setNavigationBarTitle({title:'推荐'})
  }
}
</script>

<style scoped>
.scroll-container{
  height:calc(100vh - 42px)
}
.recommend-container{
  display:flex;
  flex-wrap: wrap;
}
.recommend-item{
  width:50%;
  border: 5rpx solid  white;
}
.month-container{
  display:flex;
  color:#dd5096;
  justify-content: space-between;
  padding: 20rpx;
}
.month-info{
  font-size: 36rex;
  font-weight: 600;
  display: flex;
}
.month-date{
  display: flex;
  margin-right: 30rpx;
}
.month{
  font-size:26rpx;
}
.month-text{
  color:#4f524f;
}
.month-more{
  font-size:34rpx;
}
.month-picture{
  display:flex;
  flex-wrap: wrap;
}
.month-picture-item{
   width:33.33%;
   border: 5rpx solid  white;
}
.hot-title{
  border-left: 10rpx solid #de5999;
  padding: 10rpx;
  margin: 20rpx;
}
.hot-title text{
  margin-left: 10rpx;
  font-size: 36rpx;
}
.hot-picture{
  display: flex;
  flex-wrap: wrap;
}
.hot-picture-item{
  width: 33.33%;
  border: 5rpx solid  white;
}
</style>