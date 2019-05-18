<template>
  <div>
    <div class="search">
      <div class="search-bar">
        <icon type="search"></icon>
        <span>搜索</span>
      </div>
    </div>
    <swiper
      indicator-dots="true"
      indicator-active-color="#ffffff"
      circular="true"
      autoplay="true"
      interval="1000"
    >
      <swiper-item
        :key="index"
        v-for="(item,index) in imagesUrls"
      >
        <image
          :src="item.image_src"
          class="slide-image"
        />
      </swiper-item>
    </swiper>
    <!-- 菜单 -->
    <div class="menu">
      <div
        :key='index'
        v-for='(item, index) in menu'
        class="menu-item"
      >
        <img :src="item.image_src">
      </div>
    </div>
    <!-- 商品列表 -->
    <div
      class="floor"
      :key='index'
      v-for='(item, index) in floor'
    >
      <!-- 楼层的头部 -->
      <div class="floor-title">
        <img
          :src="item.floor_title.image_src"
          mode="aspectFill"
        >
      </div>
      <div class="floor-content">
        <div class="left">
          <img
            :src="item.product_list[0].image_src"
            mode="aspectFill"
          >
        </div>
        <div class="right">
          <img
            v-if='i>0'
            :key='i'
            v-for='(img, i) in item.product_list'
            :src="img.image_src"
            mode="aspectFill"
          >
        </div>
      </div>
    </div>
    <div
      v-if="isShow"
      @click="goTop"
      class="totop"
    >
      ︿
      <p>顶部</p>
    </div>
  </div>
</template>

<script>
import aaa from '@/utils/com'
export default {
  created() {
    this.isdata()
  },
  data() {
    return {
      isShow: false,
      imagesUrls: [],
      menu: [],
      queryData: aaa,
      floor: []
    }
  },
  methods: {
    async isdata() {
      let { data: dt } = await this.queryData('home/swiperdata');
      this.imagesUrls = dt.message;
      let { data: dit } = await this.queryData('home/catitems');
      this.menu = dit.message;
      let { data: dta } = await this.queryData('home/floordata');
      this.floor = dta.message;
    },
    // 回到顶部 
    goTop() {
      mpvue.pageScrollTo({
        scrollTop: 0,
        duration: 300
      })
    }
  },
  // 检测页面滚动事件
  onPageScroll(event) {
    // 小程序生命周期函数，监控页面的滚动
    // 如果滚动指定大小，那么就控制显示或隐藏
    this.isShow = event.scrollTop > 50
  },
  // 下拉刷新
  onPullDownRefresh() {
    this.isdata()
  }
}
</script>

<style  scoped>
.search {
  padding: 10px;
  background-color: #eb4450;
}
.search-bar {
  width: 100%;
  text-align: center;
  background-color: #fff;
}
.search-bar icon {
  vertical-align: middle;
}
.slide-image {
  width: 100%;
}
.menu {
  display: flex;
  justify-content: space-around;
}
.menu-item img {
  width: 128rpx;
  height: 140rpx;
}
.floor-content {
  display: flex;
  justify-content: space-between;
  width: 100%;
  padding: 20rpx;
  box-sizing: border-box;
}
.floor-title {
  width: 100%;
}
.floor-title img {
  width: 100%;
  height: 60rpx;
  display: block;
}
.left img {
  width: 232rpx;
  height: 385rpx;
  border-radius: 4px;
}
.right {
  flex: 1;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  margin-left: 14rpx;
}
.right img {
  width: 232rpx;
  height: 188rpx;
  border-radius: 4px;
}
.totop {
  width: 100rpx;
  height: 100rpx;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.8);
  position: fixed;
  right: 40rpx;
  bottom: 40rpx;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
