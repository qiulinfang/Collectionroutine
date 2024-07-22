<template>
	<view class="chat-container">
		<scroll-view class="message-list" scroll-y="true" :scroll-top="scrollTop"
			:style="{paddingBottom: inputBoxHeight+'rpx'}">
			<!-- 消息列表 -->
			<view class="message" v-for="(msg, index) in messages" :key="index">
				消息{{msg}}
			</view>
		</scroll-view>
		<view class="input-area" style="position: fixed; bottom: 0; left: 0; right: 0;">
			<!-- 底部输入框 -->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				messages: [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
					1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
					1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
				], // 消息数组
				scrollTop: 0, // 用于控制scroll-view滚动位置的变量
				inputBoxHeight: 100 // 输入框的高度，单位是rpx
			};
		},
	};
</script>

<style scoped>
	.chat-container {
		height: 100vh;
	}

	.message-list {
		height: 100%;
		/* 确保scroll-view占据所有空间，直到padding-bottom所预留的位置 */
	}

	.input-area {
		height: 100rpx;
		/* 输入框的高度 */
		background-color: #f8f8f8;
		/* 可以设置背景色，避免与消息列表混淆 */
	}
</style>