<template>
  <swiper @change="swiperChange" class="swiper-container">
    <swiper-item v-for="(item, index) in swiperList" :key="index">
      <view :animation="item.animation" class="slide-content">{{ item.content }}</view>
    </swiper-item>
  </swiper>
</template>

<script>
export default {
  data() {
    return {
      swiperList: [
        { content: '滑动1', animation: null },
        { content: '滑动2', animation: null },
		{ content: '滑动3', animation: null },
        // 根据需要添加更多内容
      ],
    };
  },
  methods: {
    swiperChange(e) {
      const currentIndex = e.detail.current;
      this.swiperList.forEach((item, index) => {
        const animation = uni.createAnimation({
          duration: 2000,
          timingFunction: 'ease',
        });

        animation.opacity(index === currentIndex ? 1 : 0).step();
        this.$set(this.swiperList, index, { ...item, animation: animation.export() });
      });
    },
  },
};
</script>


<style>
.swiper-container {
  width: 100%;
  height: 300px; /* 可根据实际情况调整 */
}

.slide-content {
  width: 80%;
  height: 80%;
  margin: 0 auto;
  margin-top: 200rpx;
  text-align: center;
  line-height: 210px;
  background-color: #4CD964;
}
</style>
