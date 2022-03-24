<template>
  <image :src="src" class="img">
</template>

<script>
export default {
    props:['src']
}
</script>

<style>
    .img{
        height: 400rpx;
        /* border-radius: 50%; */
    }
</style>