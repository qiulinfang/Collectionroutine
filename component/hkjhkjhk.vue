<template>
	<view class="camera-page">
		<button class="reset" @click="reset">重置</button>
		<view class="camera">
			<camera v-if="shiji === 'before'" device-position="back" flash="off" @error="error"></camera>
			<img v-if="shiji === 'after' && dao === 'photo'" class="editMovie" src="/static/20240723124604.png" alt="" />
			<video v-if="shiji === 'after' && dao === 'movie'" src="/static/20240724172958.mp4"
				style="margin-top: 555rpx;"></video>
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
		data() {
			return {
				cameraContext: null,
				tempFilePath: '',
				shiji: 'before',
				dao: 'photo',
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
					}).finally(() => {
						this.recording = false;
					});
				} else if (this.recording) {
					this.dao = 'photo';
					this.cameraContext.stopRecord();
					this.cameraContext.takePhoto({
						quality: 'high',
						success: (res) => {
							console.log('Image saved:', res.tempImagePath);
							this.tempFilePath = res.tempFilePath;
							// Save image path to state or use it as needed
						}
					});
					this.recording = false;
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
				this.shiji = 'before';
				this.dao = 'photo';
			}
		}
	}
</script>

<style lang="scss" scoped>
	.camera-page {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;


		.reset {
			position: absolute;
			top: 0;
			left: 0;
		}

		.camera {
			height: 80%;
		}

		.tool {
			height: 20%;
			background-color: rgba(0, 0, 0, 0.5);

			.button {
				width: 150rpx;
				height: 150rpx;
				border-radius: 50%;
			}
		}


	}
</style>