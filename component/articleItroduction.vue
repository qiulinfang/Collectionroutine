<template>
	<view class="generalRecommendationsPage">
		<view class="content-header">
			<img class="userinfoavatar" src="/static/images/defaultAvatar.jpg" alt="" />
			<view class="userinfomiddle">
				<view class="">
					派大星
				</view>
				<view class="">
					2022.12.12日发布
				</view>
			</view>
			<view class="userinfoRead">
				9999阅读
			</view>
		</view>
		<view class="content-middle">
			<view class="title">
				前赤壁赋
			</view>
			<view class="wenzhangneirong">
				壬戌之秋，七月既望，苏子与客泛舟游于赤壁之下。清风徐来,水波不兴。举酒属客，诵明月之..
			</view>
			<uni-grid :column="3" :highlight="true" @change="change">
				<uni-grid-item v-for="(item, index) in list" :index="index" :key="index">
					<view class="grid-item-box">
						<img class="grid-item-box-img" src="/static/images/loginAvatar.png" alt="" />
					</view>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="content-fotter">
			<view class="">
				<uni-icons type="hand-up" :size="20" color="#777" />
			
				99
			</view>
			<view class="">
				<uni-icons type="chat" :size="20" color="#777" />
				99
			</view>
			<view class="">
				<uni-icons type="paperplane" :size="20" color="#777" />
				99
			</view>
		</view>
	</view>
</template>

<script>
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
	
	.generalRecommendationsPage{
		background-color: white;
		border-radius: 10rpx;
		padding: 30rpx 20rpx;
		margin: 20rpx 0 0 0;
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
</style>