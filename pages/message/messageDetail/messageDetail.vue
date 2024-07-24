<template>
	<view class="page">
		<scroll-view class="scroll-view" scroll-y scroll-with-animation :scroll-top="scrollTop" :class="{'up': isOpen}">
			<view style="padding: 30rpx 30rpx 1rpx 30rpx;">
				<view class="message-card" v-for="(item,index) in list" :key="index">
					<view class="timestamp">
						{{convertUnixTimestampToDateString(item.timestamp)}}
					</view>
					<view class="message" :class="[item.senderId]">
						<view class="status" v-if="item.senderId === 'user_B'">
							{{item.status}}
						</view>
						<image :src="avatar" v-if="item.senderId === 'user_A'" class="avatar" mode="widthFix"></image>
						<view class="content" v-if="item.type === 'image'">
							<image :src="item.media.url" mode="widthFix"></image>
						</view>
						<view class="content" v-else> {{ item.text }} </view>
						<image :src="avatar" v-if="item.senderId === 'user_B'" class="avatar" mode="widthFix"></image>
					</view>
				</view>
			</view>
		</scroll-view>
		<view class="fsdsd">
			<view class="tool">
				<image src="/static/logo.png" mode="widthFix" class="thumb" @click="closeDrawer"></image>
				<input type="text" v-model="tempMsg.text" class="input" @confirm="send" />
				<image src="/static/logo.png" mode="widthFix" class="thumb" @click="openEmoji"></image>
				<view class="collapse-panel">
					<button @click="toggle" class="collapse-header ">
						折叠面板开关
					</button>
				</view>
			</view>
			<view v-if="emojiOpen" class="emoji-content"></view>
			<view v-if="isOpen" class="collapse-content">
				<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
					:duration="duration">
					<swiper-item>
						<view class="swiper-item uni-bg-red">
							<view class="more-icon" @click="chooseImage">
								<image  class="operation-icon" src="/static/logo.png"></image>
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
			<view class="drawer" v-if="drawerisOpen" :class="{ 'drawer--open': drawerisOpen }">
				<div class="drawer__overlay" @click="closeDrawer"></div>
				<div class="drawer__content">
					<slot></slot> <!-- 这里放置抽屉的内容 -->
					<button class="drawer__close-button" @click="closeDrawer">Close</button>
				</div>
			</view>
		</view>
	</view>
	<div class="drawer__overlay" v-if="drawerisOpen" @click="closeDrawer">
		<luyin></luyin>
	</div>
</template>
<script>
	import luyin from '/component/luyin.vue';

	export default {
		components: {
			luyin
		},
		data() {
			return {
				emojiOpen: false,
				background: ['color1', 'color2', 'color3'],
				indicatorDots: true,
				autoplay: false,
				interval: 2000,
				duration: 500,
				drawerisOpen: false,
				isOpen: false,
				content: '',
				avatar: '/static/logo.png',
				list: [{
						id: "msg_001",
						senderId: "user_A",
						receiverId: "user_B",
						text: "Hey there! How's your day?",
						media: null,
						timestamp: 1689877834000,
						read: true,
						deleted: false,
						type: "text",
						status: "seen"
					},
					{
						id: "msg_002",
						senderId: "user_B",
						receiverId: "user_A",
						text: "It's going well, thanks for asking!",
						media: null,
						timestamp: 1689878000000,
						read: true,
						deleted: false,
						type: "text",
						status: "seen"
					},
					{
						id: "msg_003",
						senderId: "user_A",
						receiverId: "user_B",
						text: null,
						media: {
							url: "/static/20240723124604.png",
							type: "image/jpeg",
							size: 123456
						},
						timestamp: 1689878100000,
						read: true,
						deleted: false,
						type: "image",
						status: "seen"
					},
					{
						id: "msg_004",
						senderId: "user_B",
						receiverId: "user_A",
						text: "Nice photo! Where did you take it?",
						media: null,
						timestamp: 1689878200000,
						read: true,
						deleted: false,
						type: "text",
						status: "seen"
					},
					{
						id: "msg_005",
						senderId: "user_A",
						receiverId: "user_B",
						text: "I took it on my last trip to the mountains.",
						media: null,
						timestamp: 1689878300000,
						read: true,
						deleted: false,
						type: "text",
						status: "seen"
					},
					{
						id: "msg_006",
						senderId: "user_B",
						receiverId: "user_A",
						text: "Sounds amazing! I'd love to see more photos.",
						media: null,
						timestamp: 1689878400000,
						read: true,
						deleted: false,
						type: "text",
						status: "seen"
					},
					{
						id: "msg_007",
						senderId: "user_A",
						receiverId: "user_B",
						text: "Sure thing, I'll send some more later.",
						media: null,
						timestamp: 1689878500000,
						read: true,
						deleted: false,
						type: "text",
						status: "seen"
					},
					{
						id: "msg_008",
						senderId: "user_B",
						receiverId: "user_A",
						text: "Great, looking forward to it!",
						media: null,
						timestamp: 1689878600000,
						read: false,
						deleted: false,
						type: "text",
						status: "delivered"
					},
					{
						id: "msg_009",
						senderId: "user_A",
						receiverId: "user_B",
						text: "By the way, have you seen any good movies lately?",
						media: null,
						timestamp: 1689878700000,
						read: false,
						deleted: false,
						type: "text",
						status: "sent"
					},
					{
						id: "msg_010",
						senderId: "user_B",
						receiverId: "user_A",
						text: "Yes, I just watched an excellent thriller. I can recommend it.",
						media: null,
						timestamp: 1689878800000,
						read: false,
						deleted: false,
						type: "text",
						status: "pending"
					}
				],
				scrollIntoViewId: '',
				tempMsg: {
					id: "msg_001",
					senderId: "user_B",
					receiverId: "user_A",
					text: "",
					media: {
						url: "",
						type: "",
						size: 0,
					},
					timestamp: Date.now(),
					read: false,
					deleted: false,
					type: "text",
					status: "pending"
				},
				scrollTop: 9999,
			};
		},
		onLoad(options) {
			uni.setNavigationBarTitle({
				title: options.name
			})
			//获取数据
		},

		methods: {
			async send() {
				this.list.push(JSON.parse(JSON.stringify(this.tempMsg)));
				this.initializeMessage();
				this.scrollToBottom()
				try {
					const response = await this.sendToServer(this.tempMsg);
					if (response.success) {
						// message.status = "sent"; // 更新消息状态
					} else {
						console.error("Failed to send message.");
					}
				} catch (error) {
					console.error("Error sending message:", error);
				};
			},
			chooseImage() {
				uni.chooseImage({
					//sourceType: 'album',
					success: (res) => {
						this.tempMsg.media.url = res.tempFilePaths[0];
						this.tempMsg.type = 'image';
						this.list.push(JSON.parse(JSON.stringify(this.tempMsg)));
						this.initializeMessage();
						this.scrollToBottom()
						// 模拟对方回复						
						// setTimeout(() => {
						// 	this.list.push({
						// 		content: '你好呀，朋友~',
						// 		userType: 'friend',
						// 		avatar: this._friendAvatar
						// 	})
						// 	this.scrollToBottom()
						// }, 1500)
					}
				})
			},
			scrollToBottom() {
				// 确保在DOM更新后设置scroll-top
				this.$nextTick(() => {
					// 将scroll-top设置为一个大数值以确保滚动到底部
					this.scrollTop++;
				});
			},
			convertUnixTimestampToDateString(timestamp) {
				return new Intl.DateTimeFormat('default', {
					year: 'numeric',
					month: '2-digit',
					day: '2-digit',
					hour: '2-digit',
					minute: '2-digit',
					second: '2-digit'
				}).format(new Date(timestamp));
			},
			sendToServer(message) {
				// 模拟发送消息到服务器的异步操作
				return new Promise((resolve, reject) => {
					setTimeout(() => {
						resolve({
							success: true,
							message
						});
					}, 1000); // 模拟网络延迟
				});
			},
			initializeMessage() {
				this.tempMsg.id = "msg_001";
				this.tempMsg.senderId = "user_B";
				this.tempMsg.receiverId = "user_A";
				this.tempMsg.text = "";
				this.tempMsg.media = {
					url: "",
					type: "",
					size: 0,
				};
				this.tempMsg.timestamp = 0;
				this.tempMsg.read = false;
				this.tempMsg.deleted = false;
				this.tempMsg.type = "text";
				this.tempMsg.status = "pending";
			},
			toggle() {
				this.isOpen = !this.isOpen;
			},
			closeDrawer() {
				this.drawerisOpen = !this.drawerisOpen;
			}
		}
	}
</script>
<style lang="scss" scoped>
	.scroll-view {
		/* #ifdef H5 */
		height: calc(100vh - 44px - 100rpx);
		/* #endif */
		/* #ifndef H5 */
		height: 100vh - 120rpx;
		/* #endif */
		background: #eee;
		box-sizing: border-box;
	}

	.scroll-view.up {
		transform: translateY(-400rpx);
	}

	.message-card {
		.timestamp {
			text-align: center;
			margin-bottom: 13rpx;
			font-size: 28rpx;
		}

		.message {
			display: flex;
			align-items: flex-start;
			margin-bottom: 30rpx;

			.status {
				font-size: 28rpx;
				align-self: flex-end;
				margin-right: 10rpx;
			}

			.avatar {
				width: 80rpx;
				height: 80rpx;
				border-radius: 10rpx;
				margin-right: 30rpx;
			}

			.content {
				box-sizing: border-box;
				min-height: 80rpx;
				max-width: 60vw;
				padding: 20rpx;
				border-radius: 10rpx;
				font-size: 28rpx;
				line-height: 1.3;
				background: #fff;
				word-break: break-all;
				/* 同上，但更现代的标准 */

				image {
					width: 200rpx;
				}
			}


			&.user_B {
				justify-content: flex-end;

				.avatar {
					margin: 0 0 0 30rpx;
				}

				.content {
					position: relative;

					&::after {
						position: absolute;
						content: '';
						width: 0;
						height: 0;
						border: 16rpx solid transparent;
						border-left: 16rpx solid #fff;
						right: -28rpx;
						top: 24rpx;
					}
				}
			}

			&.user_A {
				.content {
					position: relative;

					&::after {
						position: absolute;
						content: '';
						width: 0;
						height: 0;
						border: 16rpx solid transparent;
						border-right: 16rpx solid #fff;
						left: -28rpx;
						top: 24rpx;
					}
				}
			}
		}

	}

	.fsdsd {
		position: fixed;
		bottom: 0rpx;
		left: 0;
		right: 0;
		background-color: #fff;
		display: flex;
		flex-direction: column;
		min-height: 100rpx;
		padding-bottom: calc(20rpx + constant(safe-area-inset-bottom)/2) !important;
		padding-bottom: calc(20rpx + env(safe-area-inset-bottom)/2) !important;

		.tool {
			display: flex;
			align-items: center;


			.thumb {
				width: 64rpx;
			}

			.input {
				background: #eee;
				border-radius: 10rpx;
				height: 70rpx;
				margin-right: 30rpx;
				flex: 1;
				padding: 0 20rpx;
				box-sizing: border-box;
				font-size: 28rpx;
			}

			.collapse-panel {
				display: flex;

				.collapse-header {
					box-sizing: border-box;
					height: 100rpx;
					width: 300rpx;
					/* background-color: aquamarine; */
					padding: 0.5rem;
					cursor: pointer;
				}
			}
		}

		.collapse-content {
			box-sizing: border-box;
			height: 400rpx;
			padding: 0.5rem;
			background-color: #fff;

			.swiper {
				height: 400rpx;
				background-color: greenyellow;

				.swiper-item {
					display: block;
					height: 100rpx;
					width: 100%;
					line-height: 100rpx;
					text-align: center;
					display: grid;
					grid-template-columns: repeat(4, 1fr);
					/* 创建四列，每列占相等的空间 */
					grid-template-rows: repeat(2, auto);
					/* 创建两行，行的高度根据内容自动调整 */
					grid-gap: 10rpx;
					/* 在网格项目之间添加间距 */
					place-items: center;
					/* 水平和垂直居中所有 .grid-item */
					height: 400rpx;
					background-color: wheat;

					.more-icon {
						width: 100rpx;
						width: 100rpx;
						overflow: hidden;
						display: flex;
						flex-direction: column;
						justify-content: center;
						align-items: center;
						box-sizing: border-box;
						width: 140rpx;
						height: 140rpx;

						image {
							width: 50rpx;
							height: 50rpx;
						}
					}
				}
			}
		}
	}

	.drawer__overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.5);
	}
</style>