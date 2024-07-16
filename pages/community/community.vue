<template>
	<view class="container">
		<view class="community">
			<view class="community-header" >
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
					<view class="generalRecommendationsPage">
						<view class="content-header">
							<img class="userinfoavatar" src="/static/images/defaultAvatar.jpg" alt="" />
							<view class="userinfomiddle">
								<view class="">
									派大星
								</view>
								<view class="">
									2022.12.12日发布
								</view>
							</view>
							<view class="userinfoRead">
								9999阅读
							</view>
						</view>
						<view class="content-middle">
							<view class="title">
								前赤壁赋
							</view>
							<view class="wenzhangneirong">
								壬戌之秋，七月既望，苏子与客泛舟游于赤壁之下。清风徐来,水波不兴。举酒属客，诵明月之..
							</view>
							<uni-grid :column="3" :highlight="true" @change="change">
								<uni-grid-item v-for="(item, index) in list" :index="index" :key="index">
									<view class="grid-item-box">
										<img class="grid-item-box-img" src="/static/images/loginAvatar.png" alt="" />
									</view>
								</uni-grid-item>
							</uni-grid>
						</view>
						<view class="content-fotter">
							<view class="">
								<uni-icons type="hand-up" :size="20" color="#777" />
		
								99
							</view>
							<view class="">
								<uni-icons type="chat" :size="20" color="#777" />
								99
							</view>
							<view class="">
								<uni-icons type="paperplane" :size="20" color="#777" />
								99
							</view>
						</view>
					</view>
					<view class="generalRecommendationsPage">
						<view class="content-header">
							<img class="userinfoavatar" src="/static/images/defaultAvatar.jpg" alt="" />
							<view class="userinfomiddle">
								<view class="">
									派大星
								</view>
								<view class="">
									2022.12.12日发布
								</view>
							</view>
							<view class="userinfoRead">
								9999阅读
							</view>
						</view>
						<view class="content-middle">
							<view class="title">
								前赤壁赋
							</view>
							<view class="wenzhangneirong">
								壬戌之秋，七月既望，苏子与客泛舟游于赤壁之下。清风徐来,水波不兴。举酒属客，诵明月之..
							</view>
							<uni-grid :column="3" :highlight="true" @change="change">
								<uni-grid-item v-for="(item, index) in list" :index="index" :key="index">
									<view class="grid-item-box">
										<img class="grid-item-box-img" src="/static/images/loginAvatar.png" alt="" />
									</view>
								</uni-grid-item>
							</uni-grid>
						</view>
						<view class="content-fotter">
							<view class="">
								<uni-icons type="hand-up" :size="20" color="#777" />
		
								99
							</view>
							<view class="">
								<uni-icons type="chat" :size="20" color="#777" />
								99
							</view>
							<view class="">
								<uni-icons type="paperplane" :size="20" color="#777" />
								99
							</view>
						</view>
					</view>
				</view>
				<view class="myFollows" v-if="currentTab === 1">这里是 myFollows</view>
				<view class="confessionWall" v-if="currentTab === 2">这里是 confessionWall</view>
				<view class="professionalExchange" v-if="currentTab === 3">这里是 professionalExchange</view>
			</view>
		
		</view>
	</view>
	
</template>

<script>
	export default {
		  props: {
				list: Array,
		  },
		data() {
			return {
				navigationBarHeight:0,
				value: '',
				currentTab: 0,
				page: 1,
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
		methods: {
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
			},
			onload() {
				    const systemInfo = uni.getSystemInfoSync();
				    const statusBarHeight = systemInfo.statusBarHeight;
				    const menuButtonInfo = uni.getMenuButtonBoundingClientRect();
				    const navigationBarHeight = menuButtonInfo.height + (menuButtonInfo.top - statusBarHeight) * 2;
				
				    this.setData({
				      navigationBarHeight,
				    });
						console.log(navigationBarHeight)
			}
		}
	}
</script>

<style>
	.container{
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
		top:80rpx;
		z-index: 1000;
		background-color: azure;
	}
	
	.community-header-img{
		width: 300rpx;
	}
	
	.generalRecommendationsPage{
		background-color: white;
		border-radius: 10rpx;
		padding: 30rpx 20rpx;
		margin: 20rpx 0 0 0;
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

	.fotterimage {
		width: 100%;
		height: 100%;
	}

	.content-header {
		display: flex;
		position: relative;
	}

	.userinfoRead {
		position: absolute;
		right: 10rpx;
	}

	.title {
		font-family: 思源黑体;
		font-size: 25px;
		font-weight: bold;
		letter-spacing: 1px;

		/* 黑 */
		color: #333333;
	}

	.wenzhangneirong {
		/* 卡片-帖子-正文 */
		font-family: 思源黑体;
		font-size: 19px;
		font-weight: normal;
		text-align: justify;
		/* 浏览器可能不支持 */
		letter-spacing: 0em;

		/* 灰 */
		color: #545454;

		display: -webkit-box;
		/* 将对象作为弹性伸缩盒子模型显示 */
		-webkit-box-orient: vertical;
		/* 设置或检索伸缩盒对象的子元素的排列方式 */
		-webkit-line-clamp: 2;
		/* 显示的行数 */
		overflow: hidden;
		/* 超出的部分隐藏 */
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

	.grid-item-box-img {
		width: 100%;
		height: 100%;
	}

	.content-fotter {
		display: flex;
		flex-direction: row-reverse;
	}
</style>