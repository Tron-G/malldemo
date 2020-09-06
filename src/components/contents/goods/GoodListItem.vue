<template>
  <div class="goods-item">
    <img :src="goodsItem.show.img" alt="" />
    <div class="goods-info">
      <p>{{ goodsItem.title }}</p>
      <span class="price">
        {{ goodsItem.price }}
      </span>
      <span class="collect">{{ goodsItem.cfav }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "GoodListItem",
  props: {
    goodsItem: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  components: {},
  data() {
    return {};
  }
};
</script>

<style scoped>
.goods-item {
  position: relative;
  padding-bottom: 30px;
  text-align: center;
  /* margin-top: 10px; */
  width: 48%;
  height: auto;
  border: 2px solid #f4f4f4;
  background-color: #fff;
  border-radius: 10px;
  margin-top: 5px;
}
.goods-item img {
  width: 100%;
  height: auto;
  border-radius: 5px;
  background-size: cover;
}

.goods-info {
  font-size: 12px;
  overflow: hidden;
  padding: 5px;
  position: absolute;
  bottom: -2px;
  left: 0;
  right: 0;
}
.goods-item p {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 3px;
}
.goods-info .price {
  color: var(--color-high-text);
  position: absolute;
  left: 5px;
  font-weight: bold;
}
.price::before {
  content: "￥";
}

.goods-info .collect {
  position: relative;
  left: 65px;
}

.goods-info .collect::before {
  content: "";
  position: absolute;
  left: -15px;
  top: -1px;
  width: 14px;
  height: 14px;
  background: url("~assets/images/common/collect.svg") 0 0/14px 14px;
  background-color: #f0f5f9;
}
</style>
