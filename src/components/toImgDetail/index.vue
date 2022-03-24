<template >
    <view @click="handleClick" >
        <slot></slot>
    </view>
</template>
<script>
export default {
    props:['list','index'],
    methods:{
        handleClick(){
            getApp().globalData.list = this.list
            getApp().globalData.index = this.index
            uni.navigateTo({url:'/pages/imgDetail/index'})
          // console.log(this.list)
        }
    }
}
</script>