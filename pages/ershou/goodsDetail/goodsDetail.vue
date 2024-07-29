<template>

	<view>
		<button @click="showMessage">显示消息</button>

	</view>
	<!-- 顶部导航栏 -->
	<view class="qqq">
		<view class="share">
			<button>分享</button>
		</view>
	</view>
	<!-- 商品图片区域 -->
	<view class="qqq">
		<view class="uni-margin-wrap">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item>
					<view class="swiper-item uni-bg-red">A</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item uni-bg-green">B</view>
				</swiper-item>
				<swiper-item>
					<view class="swiper-item uni-bg-blue">C</view>
				</swiper-item>
			</swiper>
		</view>
	</view>
	<!-- 商品基本信息区 -->
	<view class="qqq">
		<view class="diyihang">
			<view class="left">
				<uni-icons type="contact" size="30"></uni-icons>
				<text>仅今日</text>
				<text>价格：11.5起</text>
				<!-- 气泡 -->
				<text>已优化14%</text>
			</view>

			<view class="right">
				已抢7885个
			</view>
		</view>
		<view class="dierhang">
			<text>平台优惠2元</text>
			<countdown :target-timestamp="targetTimestamp" />
			<text class="Drawerbg" @click='changeshowDrawer'>2件9.9折</text>
		</view>
		<view class="disanhang">
			<uni-icons type="contact" size="30"></uni-icons>
			<text @click="changeshowPopup" class='xianyonghoufu'>先用后付|支持0元下单，确认收货后再付款</text>
		</view>
		<view class="disihang">
			<text>商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字商品名字</text><button @click="changeshowPopup" class='xianyonghoufu'>坏了包赔</button>
		</view>
		<view class="diwuhang">
			<text>一周内2000+人拼单</text>
			<text>袋装</text>
			<text>肉质鲜嫩</text>
		</view>
		<view class="xianyonghoufu" @click="changeshowPopup" >
			<text>33人在拼，</text>
			<text>参与可立即拼成</text>
		</view>
		<view class="uni-margin-wrap">
			<swiper class="swiper" circular :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval"
				:duration="duration">
				<swiper-item @click="changeshowPopup">
					<view class="swiper-item uni-bg-red">A</view>
				</swiper-item >
				<swiper-item @click="changeshowPopup">
					<view class="swiper-item uni-bg-green">B</view>
				</swiper-item >
				<swiper-item @click="changeshowPopup">
					<view class="swiper-item uni-bg-blue">C</view>
				</swiper-item>
			</swiper>
		</view>
		<view class="Drawerbg" @click='changeshowDrawer'>
			<uni-icons type="contact" size="30"></uni-icons>
			<text>预计17小时内发货</text>
			<text>|承诺48小时内发货</text>
		</view>
	</view>
	<!-- 用户评价区 -->
	<view class="Drawerbg" @click='changeshowDrawer'>
		o 星级评分统计
		o 评价摘要：展示最新的用户评价。
		o 查看更多评价的链接
		(Header)</view>
	<!-- 商品选项区 -->
	<view class="qqq">
		o 规格选择：如颜色、尺寸等。
		o 数量选择器：允许用户增加或减少购买数量。
		o “立即购买”或“加入购物车”按钮
		(Header)</view>
	<!-- 商品详情区 -->
	<view class="qqq">
		<img src="/static/shangpingxiangqing/p1.jpg" alt="" />
		<img src="/static/shangpingxiangqing/p2.jpg" alt="" />
		<img src="/static/shangpingxiangqing/p3.jpg" alt="" />
		<img src="/static/shangpingxiangqing/p4.jpg" alt="" />
		<img src="/static/shangpingxiangqing/p5.jpg" alt="" />
		<img src="/static/shangpingxiangqing/p6.jpg" alt="" />
	</view>

	<!-- 推荐商品区 -->
	<view class="" v-for="(item,index) in 100" :key="index">
		{{推荐商品}}
	</view>
	<div class="product-grid">
		<div class="product-card" v-for="(item,index) in 100" :key='index' @click="navigateToDestination">
			<img src="/static/ershou/shangping.png" alt="Product Image">
			<h3>{{item.goodsName}}</h3>
			<p>商品描述</p>
		</div>
		<!-- 更多的商品卡片 -->
	</div>
	<!-- 秒杀按钮 -->
	<view style="position:fixed;bottom: 0; height: 100rpx; display:flex;background-color: lavender;">
		<view>客服</view>
		<view>更多</view>
		<view class="Drawerbg" @click='changeshowDrawer'>秒杀按钮</view>
	</view>
	<!-- 悬浮、弹出框 -->
	<Message v-if="isVisible" :message="msg" @close="hideMessage" />
	<FloatingButton v-if="showBackToTop"></FloatingButton>
	<view class="shiping"></view>
	<view class="shangmiandanmu"></view>
	<view class="xiamiandanmu"></view>
	<view class="gundongqu"></view>
	<Danmaku :messages="zhengzaipingmessages" :maxMessages="1" :showform="false" class="zhengzaiping"></Danmaku>
	<Danmaku v-if="showBackToTop" :messages="danmumessages" :maxMessages="5" class="danmu"></Danmaku>
	<movable-area class="movable-area">
		<movable-view :x="x" :y="y" direction="all" @change="onChange">
			<video class="goodVideo" src="/static/20240724172958.mp4"></video>
		</movable-view>
	</movable-area>
	<Popup :showPopup="showPopup" @hidePopup="changeshowPopup">
		<template v-slot:title-slot>
			<h3>Custom Title</h3>
		</template>
		<template v-slot:content-slot>
			<p>Custom content goes here.</p>
		</template>
	</Popup>
	<Drawer :visible = "showDrawer" @update="changeshowDrawer"></Drawer>
</template>

<script>
	import FloatingButton from '/component/FloatingButton.vue';
	import Message from '/component/Message.vue';
	import Countdown from '/component/Countdown.vue';
	import Danmaku from '/component/Danmaku.vue';
	import Popup from '/component/Popup.vue';
	import Drawer from '/component/Drawer.vue';
	
	export default {
		components: {
			FloatingButton,
			Message,
			Countdown,
			Danmaku,
			Popup,
			Drawer
		},
		data() {
			return {
				showDrawer:false,
				showPopup: false,
				danmumessages: [
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
				],
				zhengzaipingmessages: [
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
					'ssssssssssssssssss',
				],
				targetTimestamp: null,
				showBackToTop: false, // 控制按钮的显示状态
				isVisible: false,
				msg: '这是一个消息提示框。',
				background: ['color1', 'color2', 'color3'],
				indicatorDots: true,
				autoplay: false,
				interval: 2000,
				duration: 500
			};
		},
		onLoad() {
			// 设置目标时间，例如 2024 年 8 月 1 日 00:00:00
			this.targetTimestamp = new Date('2024-08-01T00:00:00').getTime();
		},
		onPageScroll: function(e) { //nvue暂不支持滚动监听，可用bindingx代替
			console.log("滚动距离为：" + e.scrollTop);
			try {
				if (e.scrollTop > 300) {
					this.showBackToTop = true;
					console.log("this.showBackToTop = true;：");

				} else {
					this.showBackToTop = false;
					console.log("this.showBackToTop = false;：");
				}
			} catch (e) {
				console.log(e)
			}
		},
		methods: {
			showMessage() {
				this.isVisible = true;
			},
			hideMessage() {
				this.isVisible = false;
			},
			changeshowPopup(){
				this.showPopup = !this.showPopup;
			},
			changeshowDrawer() {
				this.showDrawer = !this.showDrawer;
			}
		}
	}
</script>

<style scoped lang="scss">
	.qqq {
		background-color: aliceblue;
		margin-bottom: 20rpx;

		img {
			width: 100%;
		}
	}

	.uni-margin-wrap {
		width: 690rpx;
		width: 100%;

		.swiper {
			height: 300rpx;

			.swiper-item {
				display: block;
				height: 300rpx;
				line-height: 300rpx;
				text-align: center;
			}
		}

	}

	.product-grid {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		/* 定义两列 */
		gap: 1rem;
		/* 列之间的间距 */
		margin-bottom: 20rpx;

		.product-card {
			background-color: #f0f0f0;
			/* 背景颜色 */
			border-radius: 5px;
			/* 圆角 */
			box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
			/* 阴影效果 */

			img {
				width: 100%;
				/* 图片宽度与容器相同 */
				height: auto;
				object-fit: cover;
				/* 保持图片原始比例并填充容器 */
			}

			h3 {
				margin-top: 0.5rem;
				font-size: 1.2rem;
			}

			p {
				margin-top: 0.5rem;
				color: #666;
			}
		}
	}
	
	.xianyonghoufu{
		background-color: lavender;
	}

	.zhengzaiping {
		position: fixed;
		top: 200rpx;
		left: 10rpx;
	}

	.danmu {
		position: fixed;
		bottom: 200rpx;
		left: 10rpx;
	}

	.movable-area {
		position: fixed;
		top: 0;
		left: 0;
		z-index: -1;
		width: 100%;
		height: 100%;

		.goodVideo {
			width: 200rpx;
			height: 400rpx;
			border-radius: 30rpx;
		}

	}
	
	.Drawerbg{
		background-color: blue;
	}
</style>