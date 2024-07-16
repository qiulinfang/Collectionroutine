<template>
	<input v-model="article.pageTitle" class="uni-input" maxlength="10" placeholder="最大输入长度为10" />
	<textarea v-model="article.pageContent" @blur="bindTextAreaBlur" maxlength='300' auto-height />
	<view class="example-body">
		<uni-file-picker v-model="article.imageValue" fileMediatype="image" title='是的是的' limit="9" mode="grid"
			@select="select" @progress="progress" @success="success" @fail="fail"></uni-file-picker>
	</view>
	<view class="">
		<text>添加话题</text>
		<uni-data-checkbox mode="tag" multiple v-model="article.checkbox1" :localdata="hobby"></uni-data-checkbox>
	</view>

	<button type="primary" class="publish-btn" @tap="publishArticle">页面主操作 Normal</button>
</template>

<script>
	export default {
		data() {
			return {
				article: {
					pageTitle: '',
					pageContent: '',
					imageValue: [],
					checkbox1: [],
				},
				hobby: [{
					text: '综合推荐',
					value: 0
				}, {
					text: '表白墙',
					value: 3
				}, {
					text: '活动推广',
					value: 2
				}],
			}
		},
		methods: {
			async publishArticle() {
				// 发布文章前的验证逻辑（如非空验证）可以在这里添加
				if (!this.article.pageTitle || !this.article.pageContent) {
					uni.showToast({
						title: '标题和内容不能为空',
						icon: 'none'
					});
					return;
				}

				try {
					// 发送请求到后端API
					const response = await uni.request({
						url: 'http://localhost:3000/api/publishArticle', // 替换为你的后端API地址
						method: 'POST',
						header: {
							'content-type': 'application/json'
						},
						data: this.article
					});

					if (response.statusCode === 200) {
						uni.showToast({
							title: '文章发布成功',
							icon: 'success'
						});
						// 发布成功后的处理，如重置表单或跳转页面
					} else {
						uni.showToast({
							title: '文章发布失败，请稍后再试',
							icon: 'none'
						});
					}
				} catch (error) {
					console.error('发布文章时出错:', error);
					uni.showToast({
						title: '网络错误，请检查网络连接',
						icon: 'none'
					});
				}
			}
		}
	}
</script>

<style>

</style>