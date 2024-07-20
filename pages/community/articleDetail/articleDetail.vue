<template>
	<view class="pageIntroductionContainer">
		<view class="content-header">
			<img class="userinfoavatar" :src="pageList.user.avatar" alt="" />
			<view class="userinfomiddle">
				<view class="">
					{{pageList.user.userName}}
				</view>
				<view class="">
					{{pageList.article.createTime}}
				</view>
			</view>
			<button @click='changConcern'>{{mutualConcern === false ? '关注' : "未关注"}}</button>
		</view>
		<view class="content-middle">
			<view class="title" @click="goToArticleDetail">
				{{pageList.article.title}}
			</view>
			<view class="wenzhangneirong" @click="goToArticleDetail">
				{{pageList.article.content}}
			</view>
			<uni-grid :column="3" :highlight="true" @change="change">
				<uni-grid-item v-for="(pageList, index) in pageList.image" :index="index" :key="index">
					<view class="grid-item-box">
						<img class="grid-item-box-img" src="/static/images/loginAvatar.png" alt="" />
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="content-fotter">
			<view class="">
				<uni-icons type="hand-up" :size="20" color="#777" />
				{{pageList.article.likeNum}}
			</view>
			<view class="">
				<uni-icons type="chat" :size="20" color="#777" />
				{{pageList.article.commentNum}}
			</view>
			<view class="">

				<button plain="true" class="text-only-button" open-type="share"> 
				<uni-icons type="paperplane" :size="20" color="#777" open-type="share" />
					{{pageList.article.shareNum}}</button>
			</view>
		</view>
	</view>
	<view class="pinglunquyu">
		<view class="">
			一共有X条评论
		</view>
		<view class="xiangxipinglun">
			<common v-for="(item,index) in [1,2,3,4]"></common>
		</view>
	</view>
	<view class="meiyoushuju" v-if="meiyoushuju">
		没有更多数据
	</view>
	<view class="fabiaopinglun">
		<input type="text" placeholder="请输入你的评论" />
		<button>发送</button>
	</view>
</template>

<script>
	import common from '/component/common.vue';
	export default {
		components: {
			common
		},
		data() {
			return {
				pageList: {},
				mutualConcern: false,
				meiyoushuju: true,
			}
		},
		onLoad: function() {
			this.fetchData();

		},
		onShareAppMessage(res) {
			if (res.from === 'button') { // 来自页面内分享按钮
				console.log(res.target)
			}
			return {
				title: '自定义分享标题',
				path: '/pages/test/test?id=123'
			}
		},
		methods: {
			async fetchData() {
				try {
					const data1 = await uni.request({
						url: 'http://localhost:3000/IntactArticleuser-article'
					});
					this.pageList = data1.data[0];
					console.log(this.pageList)
				} catch (error) {
					console.error('请求失败:', error);
				}
			},
			changConcern() {
				this.mutualConcern = !this.mutualConcern;
			}
		}
	}
</script>

<style scoped>
	.pageIntroductionContainer {
		box-shadow: 3px 3px 2px 0 rgba(0, 0, 0, 0.1);
		background-color: white;
		border-radius: 10rpx;
		padding: 30rpx 20rpx;
		margin: 20rpx 0 0 0;
	}

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

	.meiyoushuju {
		text-align: center;
	}

	.fabiaopinglun {
		position: fixed;
		bottom: 0;
		display: flex;
		justify-content: space-between;
		width: 100%;
		background-color: white;
	}
	
	.text-only-button {
	  /* 去除默认的边框 */
	  border: none;
	  
	  /* 去除默认的背景 */
	  background: none;
	  
	  /* 设置文本颜色，可以根据需要更改 */
	  color: #333;
	  
	  /* 去除默认的内边距 */
	  padding: 0;
	  
	  /* 去除默认的外边距 */
	  margin: 0;
	  
	  /* 重置默认的字体大小和行高 */
	  font-size: inherit;
	  line-height: inherit;
	  
	  /* 可选：去除鼠标悬停时的下划线 */
	  text-decoration: none;
	  
	  /* 可选：设置光标为指针，以指示可点击 */
	  cursor: pointer;
	}
</style>