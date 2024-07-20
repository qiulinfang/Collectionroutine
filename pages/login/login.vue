<!-- pages/login/login.vue -->
<template>
  <view class="container">
    <view class="input-group">
      <input type="number" placeholder="请输入手机号" v-model="phone" />
    </view>
    <view class="input-group">
      <input type="number" placeholder="请输入验证码" v-model="captcha" />
      <button @click="sendCaptcha" :disabled="isSending">{{ captchaButtonText }}</button>
    </view>
    <button @click="login">登录</button>
  </view>
</template>

<script>
export default {
  data() {
    return {
      phone: '',
      captcha: '',
      isSending: false,
      captchaButtonText: '发送验证码'
    };
  },
  methods: {
    async sendCaptcha() {
      this.isSending = true;
      this.captchaButtonText = '发送中...';
      try {
        // 调用后端接口发送验证码
        // await this.$http.post('/api/sendCaptcha', { phone: this.phone });
        uni.showToast({ title: '验证码已发送，请查收', icon: 'none' });
      } catch (error) {
        uni.showToast({ title: error.message, icon: 'none' });
      } finally {
        this.isSending = false;
        this.captchaButtonText = '重新发送';
      }
    },
    async login() {
      // if (!this.phone || !this.captcha) {
      //   uni.showToast({ title: '手机号和验证码不能为空', icon: 'none' });
      //   return;
      // }
      try {
        const response = await uni.request({
						url:"http://localhost:3000/accountauth/?userId=1610561647732920322",
						method: 'GET',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						}
					});
        uni.setStorageSync('token', response.data[0].token); // 存储token
        // 跳转到主界面或其他页面
        uni.reLaunch({
          url: '/pages/community/community'
        });
      } catch (error) {
        uni.showToast({ title: error.message, icon: 'none' });
      }
    }
  }
};
</script>