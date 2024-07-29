<template>
	<transition name="fade">
		<div class="message-overlay" @click.self="close">
			<div class="message-box" @click.stop>
				<p>{{ message }}</p>
				<button @click="close">关闭</button>
			</div>
		</div>
	</transition>
</template>

<script>
	export default {
		name: 'Message',
		props: {
			message: {
				type: String,
				required: true
			}
		},
		methods: {
			close() {
				this.$emit('close');
			}
		}
	}
</script>

<style scoped>
	.message-overlay {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.5);
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.message-box {
		background-color: white;
		padding: 20px;
		border-radius: 5px;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
	}

	.fade-enter-active,
	.fade-leave-active {
		transition: opacity 0.3s;
	}

	.fade-enter,
	.fade-leave-to {
		opacity: 0;
	}
</style>