<template>
	<view class="container">
		<uni-easyinput prefixIcon="search" v-model="value" placeholder="左侧图标" @iconClick="handleSearchClick"
			@focus="handleSearchClick" @blur="inputBlur"  @confirm="onConfirmClick">
		</uni-easyinput>
		<view class="mohusousuo" v-if='isSearching'>
			<view class="">
				<block v-for="(item, index) in articleList" :key="item.articleId" >
					<view class="item" @click="jumpToArticlDeatile">
						<text class="xuhao"
							:class="{'first': index === 0, 'second': index === 1,'third': index === 2,'others': index > 2 }">{{index + 1}}</text>
						<text>{{item.title}}</text>
					</view>
				</block>
			</view>
			<view class="">
				已全部加载
			</view>
		</view>

		<view class="" v-if='!isSearching'>
			<view class="searchHistory" v-if="searchHistory.length !== 0">
				<view class="">
					搜索历史
				</view>
				<view class="">
					<text class="historyItem" v-for="(item, index) in searchHistory" :key="index">
						<text>{{item}}</text>
					</text>
				</view>
			</view>
			<!-- 导航栏 -->
			<view class="navbar">
				<topNavBar :categories="categories" @indexChange='changPage'></topNavBar>
			</view>

				
			<view class=""  v-if="currentIndex === 0 &&searchResult.length === 0  && afterSearch === true">
				没有找到你要搜索的内容
			</view>
			<view class=""  v-if="currentIndex === 0 &&searchResult.length !== 0  && afterSearch === true">
				<view class="">
					<view class="historyItem" v-for="(item, index) in searchResult" :key="index">
						<text>{{item}}</text>
					</view>
				</view>
			</view>
			<!-- 内容区域 -->
			<view class="content" v-if="currentIndex === 0 &&searchResult.length === 0">
				<view>
					<!-- A选项的内容 -->
					<block v-for="(item, index) in articleList" :key="item.articleId">
						<view class="item">
							<text class="xuhao"
								:class="{'first': index === 0, 'second': index === 1,'third': index === 2,'others': index > 2 }">{{index + 1}}</text>
							<text>{{item.title}}</text>
						</view>
					</block>
				</view>
			</view>
			<view class=""  v-if="currentIndex === 1 &&searchResult.length === 0  && afterSearch === true">
				没有找到你要搜索的用户
			</view>
			<view class=""  v-if="currentIndex === 1 &&searchResult.length !== 0  && afterSearch === true">
				<view class="">
					<view class="historyItem" v-for="(item, index) in searchResult" :key="index">
						<text>{{item}}</text>
					</view>
				</view>
			</view>
			<view class="content1" v-if="currentIndex === 1  &&searchResult.length === 0" >
				<view>
					<!-- B选项的内容 -->
					<block v-for="(item, index) in activeUser" :key="index">
						<view class="item1">
							<view class="item-avatar-container" @click="jumpToUserPage">
								<img class="item-avatar" :src="item.avatar" alt="" />
							</view>
							<view class="item-middle">
								<view class="item-middle-up">
									<text class="user-name" @click="jumpToUserPage">{{item.userName}}</text>
									<view class="gender-icon">
										<!-- 判断性别变量genderValue -->
										<image class="gender-icon-img" v-if="item.sex === '0'" src="/static/male.png" mode="aspectFit">
										</image>
										<image class="gender-icon-img" v-else src="/static/female.png" mode="aspectFit"></image>
									</view>
								</view>
								<view class="item-middle-down">
									<view class="fansnum">粉丝数：{{item.fansNum}}</view>
								</view>
							</view>
							<button @click='changConcern(index)'>{{item.mutualConcern === false ? '关注' : "未关注"}}</button>
						</view>
					</block>
				</view>
			</view>
		</view>
		

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
				searchHistory:[1,2],
				searchResult:[],
				isSearching: false,
				afterSearch: false,
				currentIndex: 0,
				articleList: [],
				activeUser: [],
				categories: [{
						familyId: 0,
						familyName: '文章'
					},
					{
						familyId: 1,
						familyName: '用户'
					},
				],
			}
		},
		onLoad: function(options) {
			console.log('search onload函数执行了')
			try {
				// this.fetchData();
				this.fetchData1();
				this.fetchData2();
			} catch (e) {
				console.log(e)
			}

		},
		methods: {
			async fetchData() {
				try {
					const [data1, data2] = await Promise.all([
						uni.request({
							url: 'http://localhost:3000/IntactArticlehottestRecommendation'
						}),
						uni.request({
							url: 'http://localhost:3000/useractiveUser'
						})
					]);
					console.log(data1, data2)
					this.pageList = data1.data;
					this.activeUser = data2.data;
					console.log(this.pageList, this.activeUser)
				} catch (error) {
					console.error('请求失败:', error);
				}
			},
			async fetchData1() {
				try {
					const res = await uni.request({
						url: 'http://localhost:3000/IntactArticlehottestRecommendation',
						method: 'GET',
						// 可以在这里添加headers等其他参数
					});

					// uni.request 返回的是一个对象，其中 data 是服务器返回的数据
					this.articleList = res.data;
				} catch (error) {
					// 错误处理
					console.error('请求失败:', error);
					throw error; // 可以选择抛出错误，以便调用者处理
				}
			},
			async fetchData2() {
				try {
					const res = await uni.request({
						url: 'http://localhost:3000/useractiveUser',
						method: 'GET',
						// 可以在这里添加headers等其他参数
					});

					// uni.request 返回的是一个对象，其中 data 是服务器返回的数据
					console.log(res.data)
					this.activeUser = res.data;
				} catch (error) {
					// 错误处理
					console.error('请求失败:', error);
					throw error; // 可以选择抛出错误，以便调用者处理
				}
			},
			fetchSearchResult() {
				// 返回searchResult
				this.afterSearch = true;
			},
			changPage(index) {
				this.currentIndex = index;
			},
			changConcern(index) {
				this.activeUser[index].mutualConcern = !this.activeUser[index].mutualConcern;
				// 接下来进行接口调用并fetchdata
			},
				
			inputBlur() {
				this.handleSearchClick();
				this.searchHistory.push(1);
			},
			onConfirmClick() {
				this.fetchSearchResult();
			},
			jumpToUserPage() {
				uni.navigateTo({
					url: '/pages/userPage/userPage'
				});
			},
			jumpToArticlDeatile() {
				uni.navigateTo({
					url: '/pages/community/articleDetail/articleDetail'
				});
			},
			handleSearchClick() {
				this.isSearching = !this.isSearching
			}
		},
	}
</script>

<style scoped>
	.container {
		margin: 0 20rpx;
	}

	.content {
		background-image: linear-gradient(to bottom, rgba(172, 229, 230, 0.8), rgba(238, 242, 244) 60%);
		padding: 10rpx 20rpx 50rpx 20rpx;

	}

	.item {
		display: flex;
		align-items: flex-end;
		margin: 20px 0 0rpx 0;
		font-family: 思源黑体;
		font-size: 30rpx;
		font-weight: 500;
		letter-spacing: 0px;
	}

	.xuhao {
		margin-right: 16rpx;
		border-radius: 8rpx;
		width: 30rpx;
		height: 30rpx;
		text-align: center;
		line-height: 30rpx;
		font-size: 24rpx;
		color: white;
	}

	.first {
		background-color: #EB4C39;
	}

	.second {
		background-color: #EE8041;
	}

	.third {
		background-color: #F2A748;
	}

	.others {
		background-color: #CCCCCC;
	}

	.content1 {
		background-color: white;
	}

	.item1 {
		position: relative;
		display: flex;
		align-items: center;
		width: 100%;
		height: 150rpx;
		border-bottom: solid 1px #EEF2F4;
		padding: 20rpx 10rpx;
	}

	.item-avatar {
		width: 130rpx;
		height: 130rpx;
		border-radius: 50%;
	}


	.item-middle {
		display: flex;
		flex-direction: column;
		padding-top: 30rpx;
		height: 100%;
		margin-left: 20rpx;
	}

	.item-middle-up {
		display: flex;
	}

	.user-name {
		font-family: 思源黑体;
		font-size: 30rpx;
		font-weight: 550;
		line-height: 17px;
		letter-spacing: 0px;
	}


	.gender-icon {
		margin-left: 10rpx;
	}

	.gender-icon-img {
		width: 35rpx;
		height: 35rpx;
	}

	.fansnum {
		color: #545454;
	}


	button {
		position: absolute;
		right: 30rpx;
		width: 100px;
		height: 40px;
		vertical-align: center;
		border: ;

		border-radius: 193px;
		opacity: 1;
		background-color: #3FD3D1;
		font-size: 30rpx;
		color: white;
	}
</style>