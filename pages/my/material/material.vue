<template>
	<view class="header">
		<view class="user-info">
			<view class="user-info-item" @click="chooseAvatar">
				<img :src="userAvatar" alt="" mode="aspectFill" class="user-info-avatar" />
				<img src="/static/images/change_header.png" alt="" mode="widthFix" class="changeHeaderImg" />
			</view>
			<view class="user-info-item">
				<view v-show="!editStates.userName" class="nickname-display">
					<view class="">{{editDataObj.userName}}</view>
					<uni-icons class="icon-right" custom-prefix="iconfont" type="back" color="#545454" size="28rpx"
						@click="toggleEdit('userName')">
					</uni-icons>
				</view>
				<input v-show="editStates.userName" focus='true' ref="userName" :value="editDataObj.userName" type="text"
					v-model="editDataObj.userName" style="text-align: center;" placeholder="请输入文字"
					@blur="handleBlur('userName')" />
				<view class="error-message" v-if="errorMessage">{{ errorMessage }}</view>
			</view>
			<view class="user-info-item">
				<view v-show="!editStates.userSignature" class="nickname-display">
					<view class="">{{editDataObj.userSignature}}</view>
					<uni-icons class="icon-right" custom-prefix="iconfont" type="back" color="#545454" size="28rpx"
						@click="toggleEdit('userSignature')">
					</uni-icons>
				</view>
				<input v-show="editStates.userSignature" ref="userSignature" :value="editDataObj.userSignature" type="text"
					v-model="editDataObj.userSignature" style="text-align: center;" placeholder="请输入文字"
					@blur="handleBlur('userSignature')" />
			</view>
		</view>
	</view>
	<view class="ellipse"></view>
	<view class="content">
		<view class="contentItem">
			<view class="contentItemLeft">用户ID</view>
			<view class="contentItemMiddle">
				{{editDataObj.id}}
			</view>
		</view>
		<view class="contentItem">
			<view class="contentItemLeft">性别</view>
			<view class="contentItemMiddle">
				<view v-show="!editStates.userSex" class="nickname-display">
					<view class="">{{sexList[editDataObj.userSex]}}</view>
				</view>
				<picker mode=selector :range="sexList" :value="pickerIndex" @change="confirmGender" class="picker-container"
					ref='userSex'>
					<text v-show="editStates.userSex">{{sexList[editDataObj.userSex]}}</text>
				</picker>
			</view>
			<view class="contentItemRight" @click="toggleEdit('userSex')" v-show="!editStates.userSex">
				<uni-icons class="icon-right" custom-prefix="iconfont" type="back" color="#545454" size="28rpx">
				</uni-icons>
			</view>
		</view>
		<view class="contentItem">
			<view class="contentItemLeft">手机号</view>
			<view class="contentItemMiddle">
				<view v-show="!editStates.userPhone">{{editDataObj.userPhone}}</view>
				<input v-show="editStates.userPhone" ref="userPhone" :value="editDataObj.userPhone" type="text"
					v-model="editDataObj.userPhone" style="text-align: center;" placeholder="请输入文字"
					@blur="handleBlur('userPhone')" />
			</view>
			<view class="contentItemRight" v-show="!editStates.userPhone">
				<uni-icons class="icon-right" custom-prefix="iconfont" type="back" color="#545454" size="28rpx"
					@click="toggleEdit('userPhone')">
				</uni-icons>
			</view>
		</view>
		<view class="contentItem">
			<view class="contentItemLeft">微信号</view>
			<view class="contentItemMiddle">
				<view v-show="!editStates.userWx">{{editDataObj.userWx}}</view>
				<input v-show="editStates.userWx" ref="userWx" :value="editDataObj.userWx" type="text"
					v-model="editDataObj.userWx" style="text-align: center;" placeholder="请输入文字" @blur="handleBlur('userWx')" />
			</view>
			<view class="contentItemRight" v-show="!editStates.userWx">
				<uni-icons class="icon-right" custom-prefix="iconfont" type="back" color="#545454" size="28rpx"
					@click="toggleEdit('userWx')">
				</uni-icons>
			</view>
		</view>
		<view class="buttonContainer">
			<button class="saveBtn" @click="submitMaterial">保存</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userAvatar: "/static/images/loginAvatar.png",
				errorMessage: '',
				pickerIndex: 0, // 默认选中项
				sexList: [
					// 性别选择列表
					"男",
					"女",
					"保密",
				],
				// 状态
				inputState: '',
				// 修改的数据
				editDataObj: {
					id: 2098,
					// 修改的头像
					userAvatar: "/static/images/loginAvatar.png",
					// 修改的名称
					userName: "启嘉校园",
					// 修改的个性签名
					userSignature: "www.change.tm",
					// 修改的手机号
					userPhone: "",
					// 修改的微信号
					userWx: "",
					// 修改的性别
					userSex: 0,
				},
				editStates: {
					userName: false,
					userSignature: false,
					userSex: false,
					userPhone: false,
					userWx: false
				}
			}
		},
		methods: {
			toggleEdit(field) {
				this.editStates[field] = !this.editStates[field];
				console.log(field);
				this.$nextTick(() => {
					this.$refs[field].focus();
				});
			},
			handleBlur(field) {
				switch (field) {
					case "userName":
						if (this.validateNickname()) {
							this.editStates[field] = !this.editStates[field];
							toggleEdit(field)
						}
						break;
					case "userSignature":
						this.editStates[field] = !this.editStates[field];
						break;
					case "userSex":
						this.editStates[field] = !this.editStates[field];
						break;
					case "userPhone":
						this.editStates[field] = !this.editStates[field];
						break;
					case "userWx":
						this.editStates[field] = !this.editStates[field];
						break;
				}
			},
			chooseAvatar() {
				uni.showActionSheet({
					itemList: ['拍照', '从相册选择'],
					success: (res) => {
						if (res.tapIndex === 0) {
							this.takePhoto();
						} else if (res.tapIndex === 1) {
							this.chooseFromAlbum();
						}
					}
				});
			},
			validateNickname() {
				if (this.editDataObj.userName.length < 2) {
					this.errorMessage = '你的用户昵称太短了，请换一个吧';
					return false;
				} else if (this.editDataObj.userName.length > 16) {
					this.errorMessage = '你的用户昵称太长了，换一个吧';
					return false;
				}
				this.errorMessage = ''; // 清除错误信息
				return true;
			},
			takePhoto() {
				uni.chooseImage({
					count: 1,
					sizeType: ['compressed'],
					sourceType: ['camera'],
					success: (res) => {
						this.handleImage(res.tempFilePaths[0]);
					}
				});
			},
			chooseFromAlbum() {
				uni.chooseImage({
					count: 1, // 一次选择一张图片
					sizeType: ['compressed'], // 选择压缩过的图片
					sourceType: ['album', 'camera'], // 从相册或相机选择图片
					success: (chooseImageRes) => {
						const tempFilePaths = chooseImageRes.tempFilePaths;
						if (tempFilePaths.length > 0) {
							// 获取图片的信息，包括大小
							uni.getFileInfo({
								filePath: tempFilePaths[0],
								success: (getFileInfoRes) => {
									const fileSize = getFileInfoRes.size; // 文件大小，以字节为单位
									const maxSize = 2 * 1024 * 1024; // 2MB的大小限制
									// 检查文件大小是否超过限制
									if (fileSize > maxSize) {
										// 显示警告信息
										uni.showModal({
											title: '图片大小超出限制',
											content: '头像图片大小限制为2M以内',
											success: function(res) {
												if (res.confirm) {
													console.log('用户点击确定');
												} else if (res.cancel) {
													console.log('用户点击取消');
												}
											}
										});
									} else {
										// 图片大小符合要求，可以进行下一步操作
										console.log('图片大小符合要求，可以上传或进一步处理');
										this.editDataObj.avatarUrl = tempFilePaths[0];
									}
								},
								fail: (err) => {
									console.error('获取图片信息失败', err);
								}
							});
						}
					},
					fail: (err) => {
						console.error('选择图片失败', err);
					}
				});
			},
			confirmGender(res) {
				this.editDataObj.userSex = res.detail.value;
				this.toggleEdit('userSex')
				this.showGenderPicker = false;
			},
		}
	}
</script>

<style scoped>
	.header {
		background-color: #3FD3D1;
		height: 400rpx;
	}

	.user-info {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: space-between;
	}

	.user-info-item {
		position: relative;
		display: flex;
		align-items: center;
		margin: 10rpx;
	}

	.nickname-display {
		position: relative;
		display: flex;
		align-items: center;
	}

	.changeHeaderImg {
		position: absolute;
		bottom: 0;
		right: 0;
		width: 70rpx;
		height: 70rpx;
	}

	.user-info-avatar {
		width: 222rpx;
		height: 222rpx;
		border-radius: 50%;
	}

	.error-message {
		color: red;
	}

	.ellipse {
		position: relative;
		z-index: -1;
		width: 100%;
		height: 100rpx;
		transform: translateY(-50%);
		background-color: #3FD3D1;
		border-radius: 50% / 50%;
	}

	.contentItem {
		display: flex;
		align-items: center;
		width: 100%;
		height: 100rpx;
		margin: 10rpx 30rpx 0;
	}

	.contentItemLeft {
		width: 200rpx;
	}

	.contentItemMiddle {
		width: 400rpx;
	}

	.contentItemRight {
		position: absolute;
		right: 20rpx
	}

	.buttonContainer {
		position: absolute;
		bottom: 10%;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 100%;
	}

	.saveBtn {
		width: 90%;
		background-color: #3FD3D1;
	}
</style>