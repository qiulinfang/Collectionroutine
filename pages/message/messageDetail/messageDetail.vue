<template>
	<view class="page">
		<scroll-view class="scroll-view" scroll-y scroll-with-animation :scroll-top="scrollTop">
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
		<view class="tool">
			<image src="/static/logo.png" mode="widthFix" class="thumb" @click="chooseImage"></image>
			<input type="text" v-model="tempMsg.text" class="input" @confirm="send" />
			<image src="/static/logo.png" mode="widthFix" class="thumb" @click="chooseImage"></image>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
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
			
		}
	}
</script>
<style lang="scss" scoped>
	.scroll-view {
		/* #ifdef H5 */
		height: calc(100vh - 44px - 120rpx);
		/* #endif */
		/* #ifndef H5 */
		height: 100vh - 120rpx;
		/* #endif */
		background: #eee;
		box-sizing: border-box;
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



	.tool {
		position: fixed;
		width: 100%;
		min-height: 120rpx;
		left: 0;
		bottom: 0;
		background: #fff;
		display: flex;
		align-items: flex-start;
		box-sizing: border-box;
		padding: 20rpx 24rpx 20rpx 40rpx;
		padding-bottom: calc(20rpx + constant(safe-area-inset-bottom)/2) !important;
		padding-bottom: calc(20rpx + env(safe-area-inset-bottom)/2) !important;

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

		.thumb {
			width: 64rpx;
		}
	}
</style>