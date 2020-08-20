<template>
  <div id="home">
    <nav-bar class="home-nav">
      <template v-slot:center>
        <div>购物街</div>
      </template>
    </nav-bar>
    <home-swiper
      :bannerdata="banners"
      class="swiper"
      @swiperImgLoaded="swiperImgLoad"
    ></home-swiper>
  </div>
</template>

<script>
import NavBar from "@/components/common/navbar/NavBar";
import HomeSwiper from "./childComps/HomeSwiper";
import { getHomeMultidata } from "network/home.js";
export default {
  name: "Home",
  components: {
    NavBar,
    HomeSwiper
  },
  data() {
    return {
      banners: [],
      recommends: []
    };
  },
  created() {
    this.getHomeData();
  },

  computed: {},
  methods: {
    getHomeData() {
      getHomeMultidata().then(res => {
        console.log(res);
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      });
    },

    swiperImgLoad() {}
  }
};
</script>

<style scoped>
#home {
  /* padding-top: 44px; */
  height: 100vh;
  position: relative;
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
.swiper {
  position: absolute;
  overflow: hidden;
  top: 44px;
  /* bottom: 44px; */
  right: 0;
  left: 0;
  /* height: 300px; */
}
</style>
