<template>
  <view class="content">
    <view>
      <text class="title">{{title}}</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: '这是cates页面',
      cateList: [],
    }
  },
  onLoad() {
    this._getCateList()
  },
  methods: {
    async _getCateList() {
      const res = await uni.request({
        url: 'https://api-hmugo-web.itheima.net/api/public/v1/categories',
        method: 'GET',
        data: {},
      })
      console.log(res)
      this.cateList = res[1].data.message
    },
  },
}
</script>
<style lang="less">
.content {
  .title {
    color: yellow;
  }
}
</style>
