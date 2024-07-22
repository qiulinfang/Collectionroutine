<template>
	<view class="container">
		<view class="message-card" @click="toMessageDetail" v-for="(item, index) in messagesYinshe" :key="index">
			<view class="avatar">
				<img :src="item.url" alt="User Avatar">
			</view>
			<view class="youcerongqi">
				<view class="details">
					<view class="sender">{{item.mestype}}</view>
					<view class="excerpt">{{item.msg}}</view>
				</view>
				<view v-if="item.unreadNum > 0" class="count">{{item.newunreadNum}}</view>
			</view>
		</view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				messages: [],
				loading: false,
				error: ''
			}
		},
		computed: {
			messagesYinshe() {
				return this.messages.map(item => {
					let newtype = '';
					let mesavatarurl = '';
					switch (item.type) {
						case "0":
							newtype = '系统';
							mesavatarurl = '/static/message-icon/dianzan.png';
							break;
						case "1":
							newtype = '评论回复评论'
							mesavatarurl = '/static/message-icon/pinglun.png'
							break;
						case "2":
							newtype = '点赞文章'
							mesavatarurl = '/static/message-icon/xitong.png'
							break;
						case "3":
							newtype = '评论文章'
							mesavatarurl = '/static/message-icon/pinglun.png'
							break;
						case "4":
							newtype = '点赞文章评论'
							break;
						case "5":
							newtype = '点赞商品'
							break;
						case "6":
							newtype = '评论商品'
							break;
						case "7":
							newtype = '点赞商品评论'
							break;
					}
					return ({
						...item, // 先复制所有属性
						mestype: newtype,
						url: mesavatarurl,
						newunreadNum: item.unreadNum > 99 ? '99+' : item.unreadNum, // 修改 age 属性
					})

				})
			}
		},
		onLoad: function() {
			console.log('fetchMessages执行');
			this.fetchMessages();
		},
		methods: {
			async fetchMessages() {
				console.log('fetchMessages执行');
				try {
					this.loading = true;
					const response = await uni.request({
						url: 'http://localhost:3000/chatMsg'
					});
					console.log(response.data)
					this.messages = response.data;
				} catch (error) {
					this.error = 'Failed to load messages';
				} finally {
					this.loading = false;
				}
			},
			onPullDownRefresh() {
				this.refreshMessages();
			},
			async refreshMessages() {
				try {
					uni.showLoading({
						title: '正在加载',
					});
					this.fetchMessages(); // 假设这是你的异步请求函数
				} catch (error) {
					console.error('加载失败', error);
				} finally {
					uni.hideLoading();
					uni.stopPullDownRefresh();
				}
			},
			toMessageDetail() {
				uni.navigateTo({
					url: '/pages/message/messageDetail/messageDetail',
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
		}
	}
</script>

<style scoped>
	.container {
		padding: 20rpx 20rpx;
	}

	.message-card {
		display: flex;
		align-items: center;
		padding: 16px;
		border-radius: 8px;
		margin-bottom: 16px;
	}

	.avatar {
		width: 48px;
		height: 48px;
		overflow: hidden;
		border-radius: 50%;
		margin-right: 16px;
	}

	.avatar img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.youcerongqi {
		flex: 1;
		display: flex;
		padding-bottom: 20rpx;
		border-bottom: 1px solid #ccc;
	}

	.details {}

	.sender {
		font-weight: bold;
	}

	.excerpt {
		color: #666;
	}

	.count {
		display: flex;
		justify-content: center;
		align-items: center;
		box-sizing: border-box;
		height: 50rpx;
		min-width: 50rpx;
		padding: 6rpx 10rpx;
		border-radius: 25rpx;
		margin: auto 30rpx auto auto;
		color: white;
		background-color: #F75454;
	}
</style>