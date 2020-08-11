<template>
  <view class="content">
    <!-- 轮播图 -->
    <swiper class="a" indicator-dots="true" autoplay="true" circular="true">
      <swiper-item v-for="(item, index) in swiperList" :key="index">
        <navigator
          :url="item.navigator_url"
          :open-type="item.open_type"
          class="a"
          hover-class="none"
        >
          <image class="a" :src="item.image_src" />
        </navigator>
      </swiper-item>
    </swiper>
    <!-- 分类区域 -->
    <view class="cate">
      <block v-for="(item, index) in cateItems" :key="index">
        <navigator
          url="/pages/cates/cates"
          :open-type="item.open_type"
          v-if="item.navigator_url"
          hover-class="none"
        >
          <image :src="item.image_src" />
        </navigator>
        <image v-else :src="item.image_src" />
      </block>
    </view>
    <!-- 楼层 -->
    <view class="floor-container">
      <view class="floor-item" v-for="(item, index) in floorData" :key="index">
        <image class="floor-item-title" :src="item.floor_title.image_src" />
        <view class="floor-img-box">
          <image
            class="floor-item-pic"
            v-for="(item, index) in item.product_list"
            :key="index"
            :src="item.image_src"
            :style="{width:item.image_width+'rpx'}"
            @click="goGoodsList(item.navigator_url)"
          />
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      swiperList: [],
      cateItems: [],
      floorData: [],
    }
  },
  onLoad() {
    this._getSwiperData()
    this._getCateItems()
    this._getFloorData()
  },
  methods: {
    // 获取轮播图数据
    async _getSwiperData() {
      const res = await uni.request({
        url: 'https://api-hmugo-web.itheima.net/api/public/v1/home/swiperdata',
        method: 'GET',
        data: {},
      })
      // console.log(res)
      if (res[1].statusCode !== 200) {
        return uni.showToast({
          icon: 'none',
          title: '获取数据失败',
          duration: 1500,
        })
      }
      this.swiperList = res[1].data.message
    },
    async _getCateItems() {
      const res = await uni.request({
        url: 'https://api-hmugo-web.itheima.net/api/public/v1/home/catitems',
        method: 'GET',
        data: {},
      })
      // console.log(res[1].data.message)
      this.cateItems = res[1].data.message
    },
    async _getFloorData() {
      const res = await uni.request({
        url: 'https://api-hmugo-web.itheima.net/api/public/v1/home/floordata',
        method: 'GET',
        data: {},
      })
      this.floorData = res[1].data.message
      // console.log(res)
    },
    goGoodsList(url) {
      uni.navigateTo({
        url: url,
      })
    },
  },
}
</script>

<style>
swiper {
  width: 100%;
  height: 350rpx;
}
.a,
.a {
  width: 100%;
}
.cate {
  display: flex;
  justify-content: space-around;
  width: 100%;
  margin: 40rpx 0;
}
.cate image {
  width: 128rpx;
  height: 140rpx;
}
.floor-item-title {
  height: 50rpx;
  width: 750rpx;
  display: block;
}
.floor-img-box {
  overflow: hidden;
}
.floor-item-pic {
  float: left;
  height: 190rpx;
  margin: 9rpx;
  margin-top: 0;
}
.floor-item-pic:nth-child(1) {
  height: 390rpx;
}
</style>
