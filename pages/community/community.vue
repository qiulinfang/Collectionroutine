<template>
	<view class="community">
		<view class="community-header">
				<img class="community-header-img" mode="aspectFit" src="/static/logo/logo.png" alt="" />
			<uni-easyinput prefixIcon="search" v-model="value" placeholder="左侧图标" @iconClick="handleSearchClick"
				@focus="handleSearchClick">
			</uni-easyinput>
			<topNavBar :categories="categories" @indexChange='changPage'></topNavBar>
		</view>
		<view class="content">
			<block v-for="(item, index) in filteredArticles" :key="index">
				<articleItroduction :pageData='item'></articleItroduction>
			</block>
		</view>
		<view class="xuanfu">
			<view class="xuanfu-item" @click="scrollToTop">
				<uni-icons type="arrow-up" size="30"></uni-icons>
			</view>
			<view class="xuanfu-item" @click="goToPublishPage">
				<uni-icons type="paperplane" size="30"></uni-icons>
			</view>
		</view>
	</view>
</template>

<script>
	import articleItroduction from '/component/articleItroduction.vue';
	import topNavBar from '/component/topNavBar.vue';
	export default {
		components: {
			articleItroduction,
			topNavBar
		},
		props: {
			list: Array,
		},
		data() {
			return {
				navigationBarHeight: 0,
				value: '',
				currentTab: 0,
				page: 1,
				pageList: [],
				categories: [],
			}
		},
		computed: {
			filteredArticles() {
				return this.pageList.filter(item => item.family.familyId === this.currentTab);
			},
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
							url: 'http://localhost:3000/IntactArticleuser-article'
						}),
						uni.request({
							url: 'http://localhost:3000/article-classify-label-list'
						})
					]);
					this.pageList = data1.data;
					this.categories = data2.data;
					console.log(this.categories)
				} catch (error) {
					console.error('请求失败:', error);
				}
			},
			handleSearchClick() {
				uni.navigateTo({
					url: '/pages/community/searchPage/searchPage',
					success: function(res) {
						// 跳转成功后的回调函数
						console.log('跳转成功');
					},
					fail: function(err) {
						// 跳转失败后的回调函数
						console.log('跳转失败', err);
					}
				});
			},
			toArticleDetail() {
				uni.navigateTo({
					url: '/pages/community/articleDetail/articleDetail',
					success: function(res) {
						// 跳转成功后的回调函数
						console.log('跳转成功');
					},
					fail: function(err) {
						// 跳转失败后的回调函数
						console.log('跳转失败', err);
					}
				});
			},
			scrollToTop() {
				console.log('回到顶部按钮被点击')
				uni.pageScrollTo({
					scrollTop: 0, // 设置scrollTop为0，意味着滚动到页面顶部
					duration: 300 // 动画持续时间，单位毫秒
				});
			},

			goToPublishPage() {
				console.log('navigateTo事件触发')
				uni.navigateTo({
					url: '/pages/community/publishPage/publishPage'
				});
			},
			changPage(index) {
				this.scrollToTop();
				this.currentTab = index;
				this.fetchData();
			}
		}
	}
</script>

<style scoped lang="scss">
	.community {
		background-color: azure;
		height: 100%;
		position: relative;
		padding: 0rpx 30rpx;
		height: 100%;
	}

	.community-header {
		position: sticky;
		top: 0rpx;
		z-index: 1000;
		background-color: azure;
	}
		

	.community-header-img {
		width: 300rpx;
		height: 100rpx;
	}

	.xuanfu {
		position: fixed;
		bottom: 10%;
		right: 3%;
		display: flex;
		flex-direction: column;

		.xuanfu-item {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 90rpx;
			height: 90rpx;
			border-radius: 50%;
			background-color: white;
			box-shadow: 1px 1px 1px 0 rgba(0, 0, 0, 0.1);
			margin-top: 5rpx;
		}
	}
</style>