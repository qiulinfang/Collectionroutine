<!-- 父组件 -->
<template>
	<view>
		<button @click="showPopup1">显示弹出层</button>
		<uniPopup ref="popup1">
				<button @tap="closePopup1">close</button>
				<button @tap="showPopup2">show</button>
		</uniPopup>
		<uniPopup ref="popup2">
			<view @tap="closePopup2">close</view>
			<view @tap="showPopup3">show</view>
		</uniPopup>
		<uniPopup ref="popup3">
			<view @tap="closePopup3">close</view>
		</uniPopup>
	</view>
</template>

<script>
	import uniPopup from '/component/uni-popup111.vue';

	export default {
		components: {
			uniPopup
		},
		methods: {
			showPopup1() {
				this.$refs.popup1.open();
			},
			closePopup1() {
				this.$refs.popup1.close();
			},
			showPopup2() {
				this.$refs.popup2.open();
			},
			closePopup2() {
				this.$refs.popup2.close();
			},
			showPopup3() {
				this.$refs.popup3.open();
			},
			closePopup3() {
				this.$refs.popup3.close();
			},
		}
	};
</script>