<template>
  <div class="recommend">
    <div
      v-for="(item, index) in recommends"
      :key="index"
      class="recommend-item"
    >
      <a :href="item.link">
        <img :src="item.image" alt="" />
        <div v-cloak>{{ item.title }}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "RecommendView",
  components: {},
  props: {
    recommends: {
      type: Array,
      default() {
        return [];
      }
    }
  },
  data() {
    return {};
  },
  computed: {},
  methods: {}
};
</script>

<style scoped>
.recommend {
  display: flex;
  width: 100vw;
  text-align: center;
  font-size: 12px;
  padding: 15px 0 30px;
  /* border-bottom: 8px solid #eee; */
}
.recommend-item {
  flex: 1;
}
.recommend-item img {
  width: 90%;
  margin-bottom: 5px;
  border-radius: 40px;
  /* height: 65px; */
}

[v-cloak] {
  display: none;
}
</style>
