<template>
	<view class="container" v-for="(item,index1) in pinglun" :key="index1">
		<view class="left">
			<img class="touxiang" :src="item.touxiangurl" alt="" />
		</view>
		<view class="right">
			<view class="right-1">
				<view class="right-11">
					<text>{{item.zuozhenicheng}}</text>
					<text>作者</text>
				</view>
				<view class="right-12">
					<view class="" @click="likePost(item)">
						<!-- 使用v-if和v-else来切换图标 -->
						<uni-icons type="hand-up" v-if="!item.isLiked" :size="20" color="#777" />
						<uni-icons type="hand-up-filled" v-else :size="20" color="#777" />
						<text>{{item.likeCount}}</text>
					</view>
				</view>
			</view>
			<view class="right-2">
				{{item.pinglunneirong}}
			</view>
			<view class="right-3">
				<text>{{formatCommentTime(item.pinglunshijian)}}</text>
				<text>回复</text>
			</view>
			<view class="right-4" v-if="zilunpingqu[index1]">
				<view class="container" v-for="(zipinglun, index2) in item.zipinglun" :key='index2'>
					<view class="left">
						<img class="touxiang" :src="zipinglun.touxiangurl" alt="" />
					</view>
					<view class="right">
						<view class="right-1">
							<view class="right-11">
								<text>{{zipinglun.zuozhenicheng}}</text>
								<text>作者</text>
							</view>
							<view class="right-12">
								<view class="" @click="likePost(zipinglun)">
									<!-- 使用v-if和v-else来切换图标 -->
									<uni-icons type="hand-up" v-if="!zipinglun.isLiked" :size="20" color="#777" />
									<uni-icons type="hand-up-filled" v-else :size="20" color="#777" />
									<text>{{zipinglun.likeCount}}</text>
								</view>
							</view>
						</view>
						<view class="right-2">
							{{zipinglun.pinglunneirong}}
						</view>
						<view class="right-3">
							<text>{{formatCommentTime(zipinglun.pinglunshijian)}}</text>
							<text>回复</text>
						</view>
					</view>
				</view>
			</view>
			<view class="right-5" v-if="item.zipinglun.length">
				<view class="right-51" @click="xianshizipinglun(index1)">
					——展开{{item.zipinglun.length}}条回复<uni-icons type="down" :size="20" color="#777" />
				</view>
				<view class="right-52" @click="yincangzipinglun(index1)" v-if="zilunpingqu[index1]">
					收起<uni-icons type="up" :size="20" color="#777" />
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return ({
				zilunpingqu: new Array(length).fill(0),
				neirong: [{}],
				pinglun: [{
						touxiangurl: '/static/images/loginAvatar.png',
						zuozhenicheng: '作者昵称',
						likeCount: 0,
						isLiked: false, // 是否已经点赞
						pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
						pinglunshijian: 1689859200,
						zipinglun: [{
								touxiangurl: '/static/images/loginAvatar.png',
								zuozhenicheng: '作者昵称',
								likeCount: 22,
								isLiked: false, // 是否已经点赞
								pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
								pinglunshijian: 1689859200,
							},
							{
								touxiangurl: '/static/images/loginAvatar.png',
								zuozhenicheng: '作者昵称',
								likeCount: 22,
								isLiked: false, // 是否已经点赞
								pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
								pinglunshijian: 1689859200,
							},
							{
								touxiangurl: '/static/images/loginAvatar.png',
								zuozhenicheng: '作者昵称',
								likeCount: 22,
								isLiked: false, // 是否已经点赞
								pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
								pinglunshijian: 1689859200,
							}
						]
					},
					{
						touxiangurl: '/static/images/loginAvatar.png',
						zuozhenicheng: '作者昵称',
						likeCount: 0,
						isLiked: false, // 是否已经点赞
						pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
						pinglunshijian: 1689859200,
						zipinglun: [
						]
					},
					{
						touxiangurl: '/static/images/loginAvatar.png',
						zuozhenicheng: '作者昵称',
						likeCount: 0,
						isLiked: false, // 是否已经点赞
						pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
						pinglunshijian: 1689859200,
						zipinglun: [{
								touxiangurl: '/static/images/loginAvatar.png',
								zuozhenicheng: '作者昵称',
								likeCount: 22,
								isLiked: false, // 是否已经点赞
								pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
								pinglunshijian: 1689859200,
							},
							{
								touxiangurl: '/static/images/loginAvatar.png',
								zuozhenicheng: '作者昵称',
								likeCount: 22,
								isLiked: false, // 是否已经点赞
								pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
								pinglunshijian: 1689859200,
							},
							{
								touxiangurl: '/static/images/loginAvatar.png',
								zuozhenicheng: '作者昵称',
								likeCount: 22,
								isLiked: false, // 是否已经点赞
								pinglunneirong: '评论区是软件平台中常用的社交区域，对作品的积极评价、肯定与赞美是对他人劳动的尊重。本模块中评论分为一级评论和二级评论，每条评论为左右结构。左侧为评论者头像，右侧为评论相关信息，主要包括评论者昵称、身份标识、点赞按',
								pinglunshijian: 1689859200,
							}
						]
					}
				]
			})
		},
		methods: {
			formatCommentTime(timeStr) {
				const now = Date.now(); // 当前时间，以毫秒为单位
				const diff = (now - timeStr * 1000) / 1000; // 转换为秒
				const minutes = Math.floor(diff / (1000 * 60));
				const hours = Math.floor(minutes / 60);
				const days = Math.floor(hours / 24);
				const years = Math.floor(days / 365);

				if (minutes < 3) {
					return '刚刚';
				} else if (hours < 1) {
					return `${minutes} 分钟前`;
				} else if (days < 1) {
					return `${hours} 小时前`;
				} else if (years < 1) {
					return `${days} 天前`;
				} else {
					return `${years} 年前`;
				}
			},
			likePost(comment) {
				if (comment.isLiked) {
					// 如果未点赞，则增加点赞数并更新状态
					comment.isLiked = false;
					comment.likeCount--;
					// // 执行点赞操作，例如调用后端API
					// this.$http.post('/like/post', {
					// 	postId: this.postId
					// }).then(response => {
					// 	// 处理后端响应
					// });
				} else {
					// 如果已点赞，则减少点赞数并更新状态
					comment.isLiked = true;
					comment.likeCount++;
					// 执行取消点赞操作，例如调用后端API
					// this.$http.delete('/like/post', {
					// 	postId: this.postId
					// }).then(response => {
					// 	// 处理后端响应
					// });
				}
			},
			xianshizipinglun(index) {
				this.zilunpingqu[index] = 1;
			},
			yincangzipinglun(index) {
				this.zilunpingqu[index] = 0;
			}
		},
	}
</script>

<style>
	.container {
		display: flex;
	}

	.touxiang {
		width: 150rpx;
		height: 150rpx;
	}

	.right-1 {
		display: flex;
		justify-content: space-between;
	}

	.right-5 {
		display: flex;
		justify-content: space-between;
	}
</style>