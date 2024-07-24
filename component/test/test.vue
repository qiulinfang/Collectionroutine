<!-- components/BottomDrawer/index.vue -->

<template>
	<view class="uni-margin-wrap">
		<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
			:duration="duration">
			<swiper-item>
				<view class="swiper-item uni-bg-red">
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
					<view class="more-icon">
						<image @click="sendImageMessage()" class="operation-icon" src="/static/logo.png"></image>
						<view class="operation-title">图片</view>
					</view>
				</view>
			</swiper-item>
			<swiper-item>
				<view class="swiper-item uni-bg-green">B</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		props: {
			visible: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return ({
				background: ['color1', 'color2', 'color3'],
				indicatorDots: true,
				autoplay: false,
				interval: 2000,
				duration: 500
			})
		},
		methods: {
			closeDrawer() {
				this.visible = !this.visible;
			}
		}
	};
</script>

<style scoped>
	.swiper {
		height: 400rpx;
	}

	.swiper-item {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		/* 创建四列，每列占相等的空间 */
		grid-template-rows: repeat(2, auto);
		/* 创建两行，行的高度根据内容自动调整 */
		grid-gap: 10rpx;
		/* 在网格项目之间添加间距 */
		place-items: center; /* 水平和垂直居中所有 .grid-item */
		height: 400rpx;
		background-color: wheat;
	}

	.more-icon {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		box-sizing: border-box;
		width: 140rpx;
		height: 140rpx;
	}


	image {
		width: 70rpx;
		height: 70rpx;
	}
</style>