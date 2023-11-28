<template>
	<view class="content">
		<scroll-view scroll-x="true" class="scroll-view-class" @touchstart="touchStart" @touchmove="touchMove" @touchend="touchEnd">
			<movable-view class="movable-view-class" :style="{transform: 'translateX(' + translateX + 'px) translateY(' + translateY + 'px)'}">
				<view class="view-class">111111</view>
				<view class="view-class">111111</view>
				<view class="view-class">111111</view>
				<view class="view-class">111111</view>
				<view class="view-class">111111</view>
			</movable-view>
		</scroll-view>
	</view>
</template>


<script>
	export default {
		data() {
			return {
				translateX: 0,
				translateY: 0,
				startX: 0,
				startY: 0,
				endX: 0,
				endY: 0,
				currentIndex: 0,
				itemWidth: 0,
				itemHeight: 0,
				itemList: []
			}
		},
		methods: {
			touchStart(e) {
				this.startX = e.touches[0].pageX
				this.startY = e.touches[0].pageY
			},
			touchMove(e) {
				this.endX = e.touches[0].pageX
				this.endY = e.touches[0].pageY
				let offsetX = this.endX - this.startX
				let offsetY = this.endY - this.startY
				this.translateX += offsetX
				this.translateY += offsetY
				this.startX = this.endX
				this.startY = this.endY
			},
			touchEnd(e) {
				let index = Math.round(this.translateX / this.itemWidth)
				if (index < 0) {
					index = 0
				} else if (index > this.itemList.length - 1) {
					index = this.itemList.length - 1
				}
				this.currentIndex = index
				this.translateX = this.currentIndex * this.itemWidth
				this.translateY = this.currentIndex * this.itemHeight
			}
		},
		mounted() {
			let query = uni.createSelectorQuery().in(this)
			query.select('.view-class').boundingClientRect((res) => {
				this.itemWidth = res.width
				this.itemHeight = res.height
			}).exec()
			query.selectAll('.view-class').boundingClientRect((res) => {
				this.itemList = res
			}).exec()
		}
	}
</script>

<style>
	.content{
		width: 100%;
		height: 100%;
		background-color: #ffff7f;
	}
</style>
