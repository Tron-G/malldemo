<template>
  <div>
    <swiper class="home-swiper">
      <swiper-item v-for="(item, index) in bannerdata" :key="index">
        <a :href="item.link">
          <img :src="item.image" alt="" @load="swiperImgLoaded" />
        </a>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
import { Swiper, SwiperItem } from "components/common/swiper";

export default {
  name: "HomeSwiper",
  components: {
    Swiper,
    SwiperItem
  },
  props: {
    bannerdata: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  data() {
    return {};
  },
  methods: {
    //图片加载完成提示
    swiperImgLoaded() {
      this.$emit("swiperImgLoaded");
    }
  }
};
</script>

<style scoped></style>
