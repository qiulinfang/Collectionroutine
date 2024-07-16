<template>
	<scroll-view class="scroll-view_H" scroll-x="true" @scroll="scroll" scroll-left="120" show-scrollbar="false">
		<view :class="{active: currentTab === index}" class="scroll-view-item_H" v-for="(item, index) in categories"
			:key="item.familyId" @click="changeCategory(index)">
			{{ item.familyName }}
		</view>
	</scroll-view>
</template>

<script>
	export default {
		name: 'TopNavBar',
		props: {
			categories: {
				type: Array,
			}
		},
		data() {
			return {
				currentTab: 0,
			}
		},
		methods: {
			changeCategory(index) {
				this.currentTab = index;
				this.$emit('indexChange',index);
			},
		}
	}
</script>

<style>
	.scroll-view_H {
		white-space: nowrap;
		width: 100%;
	}

	.scroll-view-item {
		height: 100rpx;
		text-align: center;
		font-size: 36rpx;
	}

	.scroll-view-item_H {
		display: inline-block;
		width: 25%;
		height: 100rpx;
		line-height: 100rpx;
		text-align: center;
		font-size: 36rpx;
	}

	.scroll-view_H .active {
		color: red;
		/* 当前选中的分类高亮显示 */
	}
</style>