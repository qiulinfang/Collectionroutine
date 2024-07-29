<template>
	<view class="ershou">
		<view class="ershou-header">
			<img class="ershou-header-img" mode="aspectFit" src="/static/logo/logo.png" alt="" />
			<uni-easyinput prefixIcon="search" v-model="value" placeholder="左侧图标" @iconClick="handleSearchClick"
				@focus="handleSearchClick">
			</uni-easyinput>
			<topNavBar :categories="categories" @indexChange='changPage'></topNavBar>
		</view>
		<view class="daohanglan">
			<view class="daohanglan-item">
				<image class="operation-icon" src="/static/ershou/img1.png"></image>
				<view class="operation-title">数码</view>
			</view>
			<view class="daohanglan-item">
				<image class="operation-icon" src="/static/ershou/img2.png"></image>
				<view class="operation-title">服饰</view>
			</view>
			<view class="daohanglan-item">
				<image class="operation-icon" src="/static/ershou/img3.png"></image>
				<view class="operation-title">书籍</view>
			</view>
			<view class="daohanglan-item">
				<image class="operation-icon" src="/static/ershou/img4.png"></image>
				<view class="operation-title">美妆</view>
			</view>
			<view class="daohanglan-item">
				<image class="operation-icon" src="/static/ershou/img5.png"></image>
				<view class="operation-title">其他</view>
			</view>
		</view>
		<div class="product-grid">
			<div class="product-card"v-for="(item,index) in shangping" :key='index' @click="navigateToDestination">
				<img src="/static/ershou/shangping.png" alt="Product Image">
				<h3>{{item.goodsName}}</h3>
				<p>商品描述</p>
			</div>
			<!-- 更多的商品卡片 -->
		</div>
	</view>
</template>

<script>
	import topNavBar from '/component/topNavBar.vue';
	export default {
		components: {
			topNavBar
		},

		data() {
			return {
				categories: [],
				shangping:[1,2],
			}
		},
		onLoad: function(options) {
			console.log('onload函数执行了')
			// 	const systemInfo = uni.getSystemInfoSync();
			// 	const statusBarHeight = systemInfo.statusBarHeight;
			// 	const menuButtonInfo = uni.getMenuButtonBoundingClientRect();
			// 	const navigationBarHeight = menuButtonInfo.height + (menuButtonInfo.top - statusBarHeight) * 2;

			// 	this.setData({
			// 		navigationBarHeight,
			// 	});
			// console.log(navigationBarHeight)

			this.fetchData();
		},
		methods: {
			async fetchData() {
				try {
					const [data1, data2] = await Promise.all([
						uni.request({
							url: 'http://localhost:3000/IntactGoodslist'
						}),
						uni.request({
							url: 'http://localhost:3000/article-classify-label-list'
						})
					]);
					this.shangping = data1.data;
					this.categories = data2.data;
					console.log(this.categories)
				} catch (error) {
					console.error('请求失败:', error);
				}
			},
			navigateToDestination() {
				uni.navigateTo({
					url: '/pages/ershou/goodsDetail/goodsDetail'
				});
			}
		}
	}
</script>

<style scoped lang="scss">
	.ershou {
		background-color: azure;
		height: 100%;
		position: relative;
		padding: 1rpx 30rpx;
		height: 100%;

		.ershou-header {
			position: sticky;
			top: 0rpx;
			z-index: 1000;
			background-color: azure;

			.ershou-header-img {
				width: 300rpx;
				height: 100rpx;
			}
		}

		.daohanglan {
			display: flex;
			justify-content: space-around;
			margin-bottom: 20rpx;

			.operation-icon {
				width: 100rpx;
				height: 100rpx;
			}

			.operation-title {
				text-align: center;
			}
		}

		.product-grid {
			display: grid;
			grid-template-columns: repeat(2, 1fr);
			/* 定义两列 */
			gap: 1rem;
			/* 列之间的间距 */
			margin-bottom: 20rpx;

			.product-card {
				background-color: #f0f0f0;
				/* 背景颜色 */
				border-radius: 5px;
				/* 圆角 */
				box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
				/* 阴影效果 */

				img {
					width: 100%;
					/* 图片宽度与容器相同 */
					height: auto;
					object-fit: cover;
					/* 保持图片原始比例并填充容器 */
				}

				h3 {
					margin-top: 0.5rem;
					font-size: 1.2rem;
				}

				p {
					margin-top: 0.5rem;
					color: #666;
				}
			}
		}
	}
</style>