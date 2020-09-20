<template>
  <div id="home">
    <nav-bar class="home-nav">
      <template v-slot:center>
        <div>购物街</div>
      </template>
    </nav-bar>

    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      :pull-up-load="true"
      @position="contentScroll"
      @pullingUp="loadMore"
    >
      <home-swiper
        :bannerdata="banners"
        class="swiper"
        @swiperImgLoaded="swiperImgLoad"
      ></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick"
      ></tab-control>
      <good-list :goods="showTabGoods" />

      <div class="refreshMsg" v-show="isShowBackTop">上拉加载更多</div>
    </scroll>

    <back-top @click.native="backToTop" v-show="isShowBackTop"></back-top>
  </div>
</template>

<script>
import NavBar from "@/components/common/navbar/NavBar";
import TabControl from "@/components/contents/tabControl/TabControl";
import GoodList from "@/components/contents/goods/GoodList";
import Scroll from "@/components/common/scroll/Scroll";
import backTop from "@/components/contents/backTop/backTop";

import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";

import { getHomeMultidata, getHomeGoods } from "network/home.js";
import { SHOWTOPBUTTONDIST } from "commonjs/const";

export default {
  name: "Home",
  components: {
    NavBar,
    TabControl,
    GoodList,
    Scroll,
    backTop,
    HomeSwiper,
    RecommendView,
    FeatureView
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] }
      },
      currentType: "pop",
      isShowBackTop: false
    };
  },
  created() {
    this.getHomeData();
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
  },
  methods: {
    /*
     事件监听
    */
    tabClick(index) {
      switch (index) {
        case 0:
          this.currentType = "pop";
          break;
        case 1:
          this.currentType = "new";
          break;
        case 2:
          this.currentType = "sell";
          break;
      }
    },
    swiperImgLoad() {},
    //回到顶部
    backToTop() {
      this.$refs.scroll.scrollTo(0, 0);
    },
    //回到顶部按钮隐藏
    contentScroll(pos) {
      this.isShowBackTop = -pos.y > SHOWTOPBUTTONDIST ? true : false;
    },
    //上拉加载更多
    loadMore() {
      setTimeout(() => {
        console.log("pull up");
        this.getHomeGoods(this.currentType);
      }, 1000);
    },
    /*
    网络请求
    */
    //轮播图和推荐数据
    getHomeData() {
      getHomeMultidata().then(res => {
        // console.log(res);
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },
    //商品数据
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;

        this.$refs.scroll.finishPullUp();
      });
    }
  },
  computed: {
    showTabGoods() {
      return this.goods[this.currentType].list;
    }
  }
};
</script>

<style scoped>
#home {
  position: relative;
  padding-top: 44px;
  height: 100vh;
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;

  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}

.content {
  position: absolute;
  left: 0;
  right: 0;
  top: 43px;
  bottom: 49px;
  overflow: hidden;
  background-color: #f4f4f4;
  /* height: calc(100% - 93px); */
  /* height: 450px; */
}

.refreshMsg {
  width: 100%;
  height: 40px;
  position: absolute;
  line-height: 40px;
  text-align: center;
  color: rgba(0, 0, 0, 0.5);
  font-style: oblique;
}
</style>
