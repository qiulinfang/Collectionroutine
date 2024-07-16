<template>
	<view class="container">
		<view class="content-header">
			<img class="userinfoavatar" :src="pageData.user.avatar" alt="" />
			<view class="userinfomiddle">
				<view class="">
					{{pageData.user.userName}}
				</view>
				<view class="">
					{{pageData.article.createTime}}
				</view>
			</view>
			<view class="userinfoRead">
				{{pageData.article.viewsNum}}阅读
			</view>
		</view>
		<view class="content-middle">
			<view class="title">
				{{pageData.article.title}}
			</view>
			<view class="wenzhangneirong">
				{{pageData.article.content}}
			</view>
			<uni-grid :column="3" :highlight="true" @change="change">
				<uni-grid-item v-for="(pageData, index) in pageData.image" :index="index" :key="index">
					<view class="grid-item-box">
						<img class="grid-item-box-img" src="/static/images/loginAvatar.png" alt="" />
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="content-fotter">
			<view class="">
				<uni-icons type="hand-up" :size="20" color="#777" />
				{{pageData.article.likeNum}}
			</view>
			<view class="">
				<uni-icons type="chat" :size="20" color="#777" />
				{{pageData.article.commentNum}}
			</view>
			<view class="">
				<uni-icons type="paperplane" :size="20" color="#777" />
				{{pageData.article.shareNum}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: {
			pageData: {
				type: Object,
				required: true,
			},
		},
		data() {},
		methods: {

		}
	}
</script>

<style>
	.content-header {
		display: flex;
		position: relative;
	}

	.userinfoavatar {
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
	}

	.userinfoRead {
		position: absolute;
		right: 10rpx;
	}

	.title {
		font-family: 思源黑体;
		font-size: 25px;
		font-weight: bold;
		letter-spacing: 1px;

		/* 黑 */
		color: #333333;
	}

	.wenzhangneirong {
		/* 卡片-帖子-正文 */
		font-family: 思源黑体;
		font-size: 19px;
		font-weight: normal;
		text-align: justify;
		/* 浏览器可能不支持 */
		letter-spacing: 0em;

		/* 灰 */
		color: #545454;

		display: -webkit-box;
		/* 将对象作为弹性伸缩盒子模型显示 */
		-webkit-box-orient: vertical;
		/* 设置或检索伸缩盒对象的子元素的排列方式 */
		-webkit-line-clamp: 2;
		/* 显示的行数 */
		overflow: hidden;
		/* 超出的部分隐藏 */
	}

	.grid-item-box-img {
		width: 100%;
		height: 100%;
	}

	.content-fotter {
		display: flex;
		flex-direction: row-reverse;
	}
</style>