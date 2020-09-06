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
  methods: {
    init() {
      this.$nextTick(() => {
        this.scroll = new BScroll(this.$refs.wrapper, {
          click: true
        });
      });
    }
  },
  mounted() {
    this.init();
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
