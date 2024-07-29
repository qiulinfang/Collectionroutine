<template>
	<transition name="slide">
		<view v-if="visible" class="background" @click="handleClick"> </view>
	</transition>
	<transition name="fade">
		<div v-if="visible" class="container">
			<slot></slot>
		</div>
	</transition>
</template>

<script>
	export default {
		data() {
			return {
				visible: false
			};
		},
		methods: {
			open() {
				this.visible = true;
			},
			close() {
				this.visible = false;
			},
			handleClick() {
				console.log('点击了handleClick')
				this.close();
			}
		}
	};
</script>

<style>
	.background {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.5);
	}

	.container {
		position: fixed;
		bottom: 0;
		height: 500rpx;
		width: 100%;
		background-color: white;
	}

	.slide-enter-active,
	.slide-leave-active {
		transition: opacity 0.5s;

	}

	.slide-enter-from,
	.slide-leave-to {
		opacity: 0;
	}


	.fade-enter-active,
	.fade-leave-active {
		transition: transform 0.3s;
	}

	.fade-enter-from,
	.fade-leave-to {

		transform: translateY(100%);
	}
</style>