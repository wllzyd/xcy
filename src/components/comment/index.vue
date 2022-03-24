<template >
    <view class="container">
        <slot></slot>
        <view v-for="item in list" :key="item.id" class="list">
            <view class="user-info">
                <view class="icon">
                    <image mode="widthFix" :src="item.user.avatar" />
                </view>
                <view class="info">
                    <view class="name">
                        {{item.user.name}}
                    </view>
                    <view class="time">
                        {{item.atime}}
                    </view>
                </view>
            </view>
            <view class="comment-container">
                <view class="comment">
                    {{item.content}}
                </view>
                <view class="rank iconfont icondianzan">{{item.size}}</view>
            </view>

        </view>
        
    </view>
</template>
<script>
export default {
    data(){
        return {

        }
    },
    props:['list']
}
</script>
<style lang="scss" scoped>
    .list{
        margin-bottom:20rpx;
        border-bottom: 15rpx solid #eeeeee;
    }
    .container{
        
        padding: 20rpx;
        .user-info{
            display: flex;
            height: 120rpx;
            
            .icon{
                width:15%;
                display: flex;
                justify-content: center;
                align-content: center;
                image{
                    width: 90%;
                }
            }
            .info{
                font-size: 24rpx;
                .name{
                    color:#ccc;
                }
                .time{

                }
            }
        }
        .comment-container{
            display: flex;
            justify-content: space-between;
            .comment{
                font-weight: 600;
                margin-left: 15%;
            }
        }
    }
</style>