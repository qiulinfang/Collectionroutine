<template>
	<view class="container">
		<view class="community">
			<view class="community-header">
				<img class="community-header-img" src="/static/logo/logo.png" alt="" />
				<uni-easyinput prefixIcon="search" v-model="value" placeholder="左侧图标" @iconClick="handleSearchClick"
					@focus="handleSearchClick">
				</uni-easyinput>
				<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="120" show-scrollbar="false">
					<view :class="{active: currentTab === index}" class="scroll-view-item_H" v-for="(item, index) in categories"
						@click="changeCategory(index)">
						{{ item.name }}
					</view>

				</scroll-view>
			</view>
			<view class="content">
				<view class="generalRecommendations" v-if="currentTab === 0">
					<block v-for="(item, index) in pageList" :key="index">
						<articleItroduction class="generalRecommendationsPage" :pageData='item'></articleItroduction>
					</block>
					<view class="myFollows" v-if="currentTab === 1">这里是 myFollows</view>
					<view class="confessionWall" v-if="currentTab === 2">这里是 confessionWall</view>
					<view class="professionalExchange" v-if="currentTab === 3">这里是 professionalExchange</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import articleItroduction from '/component/articleItroduction.vue';
	export default {
		components: {
			articleItroduction
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
				categories: [{
						name: '综合推荐',
						id: 0
					},
					{
						name: '我的关注',
						id: 1
					},
					{
						name: '表白墙',
						id: 2
					},
					{
						name: '专业交流',
						id: 3
					}
				],
				list: [{
						url: '/static/logo.png',
						text: 'Grid 1',
						badge: '0',
						type: "primary"
					},
					{
						url: '/static/logo.png',
						text: 'Grid 2',
						badge: '1',
						type: "success"
					},
					{
						url: '/static/logo.png',
						text: 'Grid 3',
						badge: '99',
						type: "warning"
					},
					{
						url: '/static/logo.png',
						text: 'Grid 4',
						badge: '2',
						type: "error"
					},
					{
						url: '/static/logo.png',
						text: 'Grid 5'
					},
					{
						url: '/static/logo.png',
						text: 'Grid 6'
					},
					{
						url: '/static/logo.png',
						text: 'Grid 7'
					},
					{
						url: '/static/logo.png',
						text: 'Grid 8'
					},
					{
						url: '/static/logo.png',
						text: 'Grid 9'
					}
				]
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
				const url = `http://localhost:3000/IntactArticleuser-article`;
				const res = await uni.request({
					url,
					method: 'GET',
					success: (res) => {
						if (res.statusCode === 200) {
							this.pageList = res.data.pageContent;
						} else {
							console.error('请求失败，状态码:', res.statusCode);
						}
					},
					fail: (err) => {
						console.error('请求失败:', err);
					},
				});

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
			changeCategory(index) {
				this.currentTab = index;
				this.page = 1;
				uni.pageScrollTo({
					scrollTop: 0, // 滚动到顶部
					duration: 300 // 动画持续时间为300毫秒
				});
				this.fetchData();
			},
		}
	}
</script>

<style>
	.container {
		background-color: azure;
		height: 100%;
	}

	.community {
		margin: 0rpx 30rpx;
		position: relative;
		height: 100%;
	}

	.community-header {
		position: sticky;
		top: 80rpx;
		z-index: 1000;
		background-color: azure;
	}

	.community-header-img {
		width: 300rpx;
	}

	.generalRecommendationsPage {
		background-color: white;
		border-radius: 10rpx;
		padding: 30rpx 20rpx;
		margin: 10rpx 0 0 0;
	}

	.scroll-view_H {
		white-space: nowrap;
		width: 100%;
	}

	.scroll-view-item {
		height: 100rpx;
		text-align: center;
		font-size: 36rpx;
	}

	.scroll-view-item_H {
		display: inline-block;
		width: 25%;
		height: 100rpx;
		line-height: 100rpx;
		text-align: center;
		font-size: 36rpx;
	}


	.scroll-view_H .active {
		color: red;
		/* 当前选中的分类高亮显示 */
	}


	.userinfoavatar {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
	}
</style>