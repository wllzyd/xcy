<template>
  <view class="container">
    <view class="tab">
      <view class="tab-title">
        <uni-segmented-control :current="current" :values="items" @clickItem="onClickItem" styleType="text" activeColor="#dd5096"></uni-segmented-control>
      </view>
      
    </view>
        <view class="content">
            <scroll-view class="list" scroll-y @scrolltolower="handleToLower" enable-flex>
                <view class="list-item" v-for="(item,index) in list" :key="item.id" >
                    <to-img-detail :list='list' :index='index' >
                        <image mode="widthFix" :src='item.thumb' />
                    </to-img-detail>
                </view>
            </scroll-view>
        </view>
  </view>
</template>

<script>
import { uniSegmentedControl} from "@dcloudio/uni-ui";
import toImgDetail from '@/components/toImgDetail';
export default {
  data() {
    return {
      current: 0,
      items: ["最新", "热门"],
      params:{
          limit:30,
          order:'new',
          skip:0
      },
      id:0,
      list:[],
      hasmore:true
    };
  },
  components: {
    uniSegmentedControl,
    toImgDetail
  },
  methods: {
    onClickItem(e) {
      if (this.current != e.currentIndex) {
          this.list=[]
        this.current = e.currentIndex;
        this.params.limit=30
        this.params.skip=0
        this.getList()
      }
    },
    getList(){
        this.request({
            url: `https://service.picasso.adesk.com/v1/vertical/category/${this.id}/vertical`,
            data:{...this.params,order:this.current == 0?'new':'hot'}
        }).then(res=>{
            console.log(res)
            if(res.data.res.vertical.length == 0){
                this.hasmore = false;
                return
            }
            this.list = [...this.list,...res.data.res.vertical]
        })
    },
    handleToLower(){
        if(this.hasmore){
            this.params.skip+=this.params.limit
            this.getList()
        }else{
            uni.showToast({title:'给不了更多了',icon:'none'})
        }
    }
  },
  onLoad(props) {
    this.id=props.id
    this.getList()
  },
};
</script>

<style scoped lang="scss">
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
    .list{
        display: flex;
        flex-wrap: wrap;
        height:calc(100vh - 42px);
        .list-item{
            width: 33.33%;
            border:5rpx solid #fff;
        }
    }
</style>