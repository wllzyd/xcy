<template >
    <scroll-view class="scroll-view" scroll-y v-if="album.cover">
        <view class="top">
            <image class="cover" :src="album.cover" nodel="aspectFill"/>
            <view class="info">
                <view class="title">{{album.name}}</view>
                <view class="btn">关注专辑</view>
            </view>
        </view>
        <view class="author">
            <view class="author-info">
                <image mode="widthFix" :src="album.user.avatar"  />
                <view class="name">{{album.user.name}}</view>
            </view>
            <view class="desc">
                <text>{{album.desc}}</text>
            </view>
        </view>
        <view class="picture-list">
            <view class="picture-item" v-for="item in wallpaper" :key="item.id" >
                <image mode="widthFix" :src="item.thumb+item.rule.replace('$<Height>',360)" />
            </view>
        </view>
    </scroll-view>
</template>
<script>
export default {
    data(){
        return {
            id:-1,
            params:{
                limit:30,
                order:'new',
                skip:0,
                first:1
            },
            album:{},
            wallpaper:[]
        }
    },
    methods:{
        
        getAlbumInfo(){
            this.request({
              url: `https://service.picasso.adesk.com/v1/wallpaper/album/${this.id}/wallpaper`,
              data:this.params
            }).then(res=>{
                this.album = res.data.res.album
                this.wallpaper = res.data.res.wallpaper
                if(this.wallpaper.length==0){
                    this.mockWallpaper()
                }
            })
        },
        mockWallpaper(){
            let tmp =[]
            for(let i =0;i<10;i++){
                tmp.push({id:new Date().getTime(),thumb:this.album.cover,rule:''})
            }
            this.wallpaper = [...this.wallpaper,...tmp]
        }
    },
    onLoad(props){
        this.id = props.id
        this.getAlbumInfo()
    },
    onReachBottom(){
        uni.showToast({title:'没有更多数据了',icon:'none'})
    }
}
</script>
<style lang="scss" scoped>
    .top{

        position:relative;
        .cover{

        }
        .info{
            position: absolute;
            display: flex;
            width: 100%;
            left: 0;
            bottom: 0;
            justify-content: space-between;
            align-items: center;
            height: 80rpx;
            color: #FFF;
            padding: 0 10rpx;
            .title{
                font-size:40rpx;
            }
            .btn{
                background: $color;
                width: 152rpx;
                height: 50rpx;
                display: flex;
                justify-content: center;
                align-items: center;
                border-radius: 10rpx;
            }
        }
    }
    .author{
        padding:10rpx;
    }
    .author-info{
        display: flex;
        padding:10rpx;
        image{
            width: 50rpx;
        }
        .name{
            margin-left: 15rpx;
        }
    }
    .picture-list{
        display: flex;

        flex-wrap: wrap;
        .picture-item{
            width:50%;
            border:5rpx solid #FFF;
           
        }
    }
</style>