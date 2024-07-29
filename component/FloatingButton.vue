<template>
	<div class="floating-button" @click="backToTop">
		<slot>+</slot>
	</div>
</template>

<script>
	export default {
		methods: {
			backToTop() {
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 300
				});
			}
		}
	};
</script>


<style scoped>
	.floating-button {
		position: fixed;
		bottom: 16px;
		/* 距离底部的距离 */
		right: 16px;
		/* 距离右边的距离 */
		width: 56px;
		/* 按钮大小 */
		height: 56px;
		background-color: #039be5;
		/* 背景颜色 */
		border-radius: 50%;
		/* 圆形按钮 */
		display: flex;
		align-items: center;
		justify-content: center;
		color: white;
		/* 文字颜色 */
		font-size: 24px;
		cursor: pointer;
		transition: transform 0.2s ease-in-out;
		/* 动画效果 */
	}s

	.floating-button:hover {
		transform: scale(1.1);
		/* 鼠标悬停时放大 */
	}
</style>