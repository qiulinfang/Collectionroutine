<template>
	<view class="camera-page">
		<view class="reset">
			<button @click="reset">重置</button>
			<button @click=" this.$emit('message-sent');">关闭</button>
		</view>

		<view class="camera">
			<camera class="cameraitem" v-if="shiji === 'before'" device-position="back" flash="off" @error="error"></camera>
			<img v-if="shiji === 'after' && dao === 'photo'" class="cameraitem" src="/static/20240723124604.png" alt="" />
			<video v-if="shiji === 'after' && dao === 'movie'" class="cameraitem" src="/static/20240724172958.mp4"
				style="margin-top: 100rpx;"></video>
		</view>
		<view class="tool">
			<button v-if="shiji === 'before'" class="button" @touchstart="onButtonTouchStart" @touchend="onButtonTouchEnd"
				@touchmove="onButtonTouchMove">拍照</button>
			<view v-if="shiji === 'after' && dao === 'photo'" class="editPhoto">
				<text>相片编辑功能1</text>
				<text>相片编辑功能1</text>
				<text>相片编辑功能1</text>
				<text>相片编辑功能1</text>
				<text>相片编辑功能1</text>
				<text>相片编辑功能1</text>
				<text>相片编辑功能1</text>
				<text>相片编辑功能1</text>
			</view>
			<view v-if="shiji === 'after' && dao === 'movie'" class="editMovie">
				<text>录像编辑功能1</text>
				<text>录像编辑功能1</text>
				<text>录像编辑功能1</text>
				<text>录像编辑功能1</text>
				<text>录像编辑功能1</text>
				<text>录像编辑功能1</text>
				<text>录像编辑功能1</text>
				<text>录像编辑功能1</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			kaishipaishe: Boolean,
			require: true,
		},
		data() {
			return {
				cameraContext: null,
				tempFilePath: '',
				shiji: 'before',
				dao: 'photo',
				cameraContext: null,
				touchStartTime: 0,
				zoomLevel: 0,
				recording: false
			};
		},
		methods: {
			onCameraReady(e) {
				this.cameraContext = uni.createCameraContext(e.detail.deviceId);
			},
			onButtonTouchStart(e) {
				this.touchStartTime = Date.now();
				this.recording = true;
				this.cameraContext.startRecord();
			},
			onButtonTouchEnd(e) {
				this.shiji = 'after';
				const touchEndTime = Date.now();
				const touchDuration = touchEndTime - this.touchStartTime;

				if (this.recording && touchDuration >= 1000) {
					this.dao = 'movie';
					this.cameraContext.stopRecord().then(res => {
						console.log('Video saved:', res.tempFilePath);
						this.tempFilePath = res.tempFilePath;
						// Save video path to state or use it as needed
						this.$emit('getVideotempImagePath', this.tempFilePath)
					}).finally(() => {
						this.recording = false;
					});
				} else if (this.recording) {
					this.dao = 'photo';
					this.cameraContext.stopRecord().then(() => {
						this.cameraContext.takePhoto({
							quality: 'high'
						}).then(res => {
							console.log('Image saved:', res.tempImagePath);
							this.tempFilePath = res.tempFilePath;
							this.$emit('getPhototempImagePath', this.tempFilePath)
							// Save image path to state or use it as needed
						});
					}).finally(() => {
						this.recording = false;
					});
				}
			},
			onButtonTouchMove(e) {
				if (this.recording) {
					// Calculate the zoom level based on touch movement
					const delta = e.touches[0].pageY - e.changedTouches[0].pageY;
					this.zoomLevel += delta / 100; // Adjust sensitivity as needed
					this.cameraContext.setZoom(this.zoomLevel);
					
				}
			},
			reset() {
				this.tempFilePath = '';
				this.tempFilePath = '';
				this.shiji = 'before';
				this.dao = 'photo';
			}
		}
	}
</script>

<style lang="scss" scoped>
	.camera-page {
		width: 100%;
		height: 100%;
		background-color: grey;

		.reset {
			position: absolute;
			top: 0;
			left: 0;
			display: flex;
		}

		.camera {
			height: 80%;

			.cameraitem {
				height: 100%;
				width: 100%;
			}
		}

		.tool {
			height: 20%;

			.button {
				width: 150rpx;
				height: 150rpx;
				border-radius: 50%;
			}
		}


	}
</style>