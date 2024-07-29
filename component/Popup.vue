<template>
	<transition name="fade">
		<div v-if="showPopup" class="popup" @click.self="hidePopup">
			<div class="popup-content">
				<slot name="title-slot">
					<h3>Hello!</h3>
				</slot>
				<slot name="content-slot">
					<p>This is a simple popup message.</p>
				</slot>
				<slot name="actions-slot">
					<button @click="hidePopup">Close</button>
				</slot>
			</div>
		</div>
	</transition>
</template>

<script>
	export default {
		props: {
			showPopup: {
				type: Boolean,
				default: false,
			}
		},
		data() {
			return {
			};
		},
		methods: {
			hidePopup() {
				this.$emit('hidePopup')
			},
		},
	};
</script>

<style scoped>
	.popup {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: rgba(0, 0, 0, 0.5);
	}

	.popup-content {
		background-color: #fff;
		padding: 20px;
		border-radius: 5px;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
	}

	.fade-enter-active,
	.fade-leave-active {
		transition: opacity 0.1s;
	}

	.fade-enter-from,
	.fade-leave-to {
		opacity: 0;
	}
</style>