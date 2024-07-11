<template>
	<view class="my-page">
		<view class="header">
			<view class="user-info">
				<view class="user-info-left-box">
					<image :src="userAvatar" fit="aspectFill" class="user-info-avatar"></image>
					<view class="login-container" v-if="!userObj.id">
						<text class="login-btn">未登录</text>
					</view>
					<view class="id-num-box" v-else>
						<view class="user-name">齐家网</view>
						<view class="user-ids">ID：100019</view>
					</view>
				</view>
				<view class="user-info-right-box">
					<text class="text">个人资料</text>
					<uni-icons class="right-icon" type="right"></uni-icons>
				</view>
			</view>
			<view class="user-data">
				<view>
					<view class="user-data-count">0</view>
					<view>我的文章</view>
				</view>
				<view>
					<view class="user-data-count">0</view>
					<view>我的文章</view>
				</view>
				<view>
					<view class="user-data-count">0</view>
					<view>我的文章</view>
				</view>
			</view>
		</view>
		<view class="ellipse">

		</view>
		<view class="my-functions">
			<block v-for="(item,index) in contentNavList" :key="index">
				<view @click="handleJumpPage(item.url)">
					<uni-icons custom-prefix="iconfont" :type="item.icon" size="24"></uni-icons>
					<text>{{item.text}}</text>
				</view>
			</block>
			<view @click="open">
				<uni-icons custom-prefix="iconfont" type="weixin" size="24"></uni-icons>
				<text>联系我们</text>
			</view>
		</view>
	</view>
	<!-- 退出登录 -->
	<view class="exit-container" v-if="userObj.id">
		<view class="exit-btn" @click="handlerExitLogin">
			退出登录
		</view>
	</view>
	<uni-popup ref="popup" :mask-click="true" style="position: relative;">
		<view class="contact-us">
			<image class="contact-us-logo" src="/static/cat.png" mode="widthFix"></image>
			<view class="contact-us-content">
				<view class="contact-us-content-title">
					<text>联系我们</text>
				</view>
				<view class="">
					<text>QQ：sldkfjlskdjf</text>
				</view>
				<view class="">
					<text>微信：sldkfjlskdjf</text>
				</view>
			</view>
		</view>
	</uni-popup>




</template>

<script>
	export default {
		data() {
			return {
				userAvatar: "/static/images/loginAvatar.png",
				userObj: {
					id: 'sljdf',
				}, // 用户对象
				headerNavList: [{ // 头部导航列表
						text: "我的文章",
						url: "/pages/my/issue/issue",
						num: 0
					},
					{
						text: "我的关注",
						url: "/pages/my/attention/attention",
						num: 0
					},
					{
						text: "我的粉丝",
						url: "/pages/my/fans/fans",
						num: 0
					}
				],
				contentNavList: [{ // 内容导航列表
						text: "商品管理",
						url: "/pages/my/goods/goods",
						icon: "headphones"
					},
					{
						text: "账号认证",
						url: "/pages/my/approve/approve",
						icon: "headphones"
					},
					{
						text: "推广有礼",
						url: "/pages/my/promotion/promotion",
						icon: "headphones"
					},
					{
						text: "意见反馈",
						url: "/pages/my/feedback/feedback",
						icon: "headphones"
					}
				],
			}
		},
		methods: {
			open() {
				this.$refs.popup.open('center')
			},
			close() {
				this.$refs.popup.close()
			}
		}
	}
</script>

<style lang="scss">
	.header {
		background-color: #3FD3D1;
	}

	.user-info-avatar {
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
	}

	.user-info {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.user-info-left-box {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}


	.id-num-box {
		margin-left: 20rpx;
	}

	.user-data {
		display: flex;
		justify-content: space-around;
		margin-top: 50rpx ;
		margin-right: 50rpx ;
		margin-left: 50rpx ;

		.user-data-count {
			margin-bottom: 20rpx;
			text-align: center;
		}
	}

	.contact-us {
		text-align: center;
	}

	.contact-us-logo {
		width: 400rpx;
		margin-right: 50rpx;
	}

	.ellipse {
		position: relative;
		z-index: -1;
		width: 100%;
		height: 100rpx;
		transform: translateY(-50%);
		background-color: #3FD3D1;
		border-radius: 50% / 50%;
	}

	.my-functions {
		width: 100%;
		background: #ffffff;
		padding: 0rpx 32rpx;
		box-sizing: border-box;

		.icon {
			width: 48rpx;
			height: 48rpx;
		}

		view {
			display: flex;
			align-items: center;
			border-bottom: 1px solid #EEF2F4;
			padding: 32rpx 0rpx 32rpx 0rpx;

			image {
				width: 48rpx;
			}

			text {
				color: #333333;
				margin-left: 36rpx;
				font-size: 28rpx;
			}

			&:last-child {
				border-bottom: 0px solid #EEF2F4;
			}
		}

	}

	.contact-us-content {
		background-color: white;
		position: relative;
		bottom: 50rpx;
		z-index: -1;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
		box-sizing: border-box;
		width: 500rpx;
		height: 300rpx;
		border-radius: 10rpx;
		padding: 50rpx;
	}

	//退出登录
	.exit-container {
		padding: 0rpx 32rpx;
		box-sizing: border-box;
		width: 100%;
		padding-top: 300rpx;
		padding-bottom: 60rpx;

		.exit-btn {
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 96rpx;
			border-radius: 4px;
			background: #F75454;
			color: #FFFFFF;
			font-size: 32rpx;

		}
	}
</style>