<!-- 录音按钮组件 -->
<template>
	<view :class="['record-button', { 'recording': isRecording, 'canceled': canceled }]" @touchstart="handleTouchStart"
		@touchmove="handleTouchMove" @touchend="handleTouchEnd">
		{{ recordingStatus }}
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isRecording: false,
				recordingStatus: '长按开始录音',
				touchStartY: 0,
				touchEndY: 0,
				slideDistance: 0,
				threshold: 50, // 滑动距离阈值，单位像素
				canceled: false,
				recorderManager: uni.getRecorderManager(),
				recordingTask: null,
			};
		},
		methods: {
			handleTouchStart(e) {
				this.canceled = false; // 重置取消状态
				this.touchStartY = e.touches[0].clientY;
				this.isRecording = true;
				this.recordingStatus = '正在录音';
				this.startRecording();
			},

			handleTouchMove(e) {
				if (!this.isRecording) return;
				this.touchEndY = e.touches[0].clientY;
				this.slideDistance = Math.abs(this.touchEndY - this.touchStartY);

				if (this.slideDistance > this.threshold && this.touchEndY < this.touchStartY) {
					this.recordingStatus = '松开按钮取消录音';
					this.canceled = true;
				} else {
					this.recordingStatus = '正在录音';
					this.canceled = false;
				}
			},

			handleTouchEnd(e) {
				this.touchEndY = e.changedTouches[0].clientY;
				this.handleTouchCancel(e); // 使用同一函数处理结束和取消
			},

			handleTouchCancel(e) {
				this.isRecording = false;
				this.slideDistance = Math.abs(this.touchEndY - this.touchStartY);

				if (this.slideDistance <= this.threshold) {
					this.recordingTask && this.recordingTask.stop();
					this.recordingStatus = '长按开始录音';
					this.onRecordingStopped();
				} else {
					this.recordingTask && this.recordingTask.abort();
					this.recordingStatus = '长按开始录音';
					this.canceled = false;
					this.onRecordingCanceled();
				}
			},

			startRecording() {
				this.recordingTask = this.recorderManager.start({
					duration: 60000, // 设置录音时长上限，例如60秒
				});
				this.recordingTask.onStart(() => {
					console.log('录音开始');
				});
			},

			onRecordingStopped() {
				this.recordingTask.onStop((res) => {
					console.log('录音结束', res);
					// 发送录音文件到服务器或其他处理
					this.recordingStatus = '录音已保存';
				});
			},

			onRecordingCanceled() {
				this.recordingTask.onStop(() => {
					console.log('录音被取消');
					this.recordingStatus = '录音已取消';
				});
			}
		},
	};
</script>

<style scoped>
	.record-button {
		width: 100%;
		height: 100px;
		background-color: #f0f0f0;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 20px;
		transition: background-color 0.3s;
	}

	.recording {
		background-color: #ffcc00;
	}

	.canceled {
		background-color: #ff0000;
	}
</style>