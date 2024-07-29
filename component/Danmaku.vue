<template>
	<div class="danmaku-container">
		<div class="danmaku-list" ref="danmakuList">
			<view class="danmaku" v-for="(message, index) in messages" :key="index">
				{{ message }}
			</view>
		</div>
		<view v-if="showform">
			<input v-model="newMessage" placeholder="Type your message here...">
			<button @click="addDanmaku">Send</button>
		</view>

	</div>
</template>

<script>
	export default {
		props:{
			messages:{
				type: Array,
			},
			maxMessages:{
				type: Number,
			},
			showform:{
				type: Boolean,
			}
		},
		data() {
			return {
				newMessage: '',
			};
		},
		mounted: function() {
			this.checkLimit();
		},
		methods: {
			addDanmaku() {
				try{
					if (this.newMessage.trim()) {
						console.log('open')
						this.messages.push(this.newMessage);
						this.newMessage = ''; // 清空输入框
						this.checkLimit();
					}
				}catch(e){
					console.log(e);
				}
				
			},
			checkLimit() {
				while (this.messages.length > this.maxMessages) {
					console.log('移出一条')
					this.messages.shift(); // 移除最老的弹幕
				}
			},
		}
	};
</script>

<style scoped>
	.danmaku-container {
	}

	.danmaku-list {

	}

	.fade-enter-active,
	.fade-leave-active {
		transition: opacity 0.5s;
	}

	.fade-enter-from,
	.fade-leave-to {
		opacity: 0;
	}
</style>