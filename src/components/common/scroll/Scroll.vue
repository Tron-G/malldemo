<template>
  <div class="wrapper" ref="wrapper">
    <div class="scrollcontent">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from "@better-scroll/core";
import Pullup from "@better-scroll/pull-up";

BScroll.use(Pullup);

export default {
  name: "Scroll",
  data() {
    return {
      scroll: null
    };
  },
  props: {
    probeType: {
      type: Number,
      default: 0
    },
    pullUpLoad: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    init() {
      this.$nextTick(() => {
        this.scroll = new BScroll(this.$refs.wrapper, {
          click: true,
          probeType: this.probeType,
          pullUpLoad: this.pullUpLoad
        });

        this.scroll.on("scroll", pos => {
          this.$emit("position", pos);
        });

        this.scroll.on("pullingUp", () => {
          this.$emit("pullingUp");
        });
      });
    },
    scrollTo(x, y, time = 500) {
      this.scroll.scrollTo(x, y, time);
    },
    //准备下一次上拉加载并且刷新
    finishPullUp() {
      this.scroll.finishPullUp();
      this.$nextTick(() => {
        this.scroll.refresh();
      });
    }
  },
  mounted() {
    this.init();
    //主动刷新
    setTimeout(() => {
      this.$nextTick(() => {
        this.scroll.refresh();
      });
    }, 500);
  },
  beforeDestroy() {
    this.scroll.destroy();
  }
};
</script>

<style scoped></style>
