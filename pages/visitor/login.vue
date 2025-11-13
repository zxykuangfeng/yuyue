<template>
  <view class="page">
 
    <view class="card">  
	 <image class="logo" src="/static/icon.png" mode="widthFix" />
      <button class="wechat-button" @click="handleWechatLogin">微信授权登录</button>
      <text class="skip" @click="skipLogin">暂不登录</text>
      <radio-group class="agreement" name="agreement" @change="onAgreementChange">
        <label class="agreement__item">
          <radio color="#2d8cf0" value="accepted" :checked="agreementAccepted" />
          <text class="agreement__text">登录即代表接受《用户协议》及《隐私协议》</text>
        </label>
      </radio-group>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      agreementAccepted: false,
    };
  },
  methods: {
    handleWechatLogin() {
      if (!this.agreementAccepted) {
        uni.showToast({ title: '请先阅读并同意协议', icon: 'none' });
        return;
      }
      // 这里可以放微信登录逻辑，比如 wx.login()
      uni.showToast({ title: '微信授权成功', icon: 'success' });
      setTimeout(() => {
        uni.redirectTo({ url: '/pages/visitor/center' });
      }, 500);
    },
    submit() {
      if (!this.formData.phone || !this.formData.code) {
        uni.showToast({ title: '请完善登录信息', icon: 'none' });
        return;
      }
      uni.showToast({ title: '登录成功', icon: 'success' });
      setTimeout(() => {
        uni.redirectTo({ url: '/pages/visitor/center' });
      }, 500);
    },
    skipLogin() {
      uni.showToast({ title: '已返回首页', icon: 'none' });
      setTimeout(() => {
        uni.reLaunch({ url: '/pages/index/index' });
      }, 400);
    },
    onAgreementChange({ detail }) {
      const { value } = detail;
      this.agreementAccepted = Array.isArray(value) ? value.length > 0 : !!value;
    },
  },
};
</script>

<style scoped>
.page {
  min-height: 100vh;
  padding: 120rpx 48rpx 64rpx;
  box-sizing: border-box;
  background: linear-gradient(180deg, #ebf4ff, #f8fbff 45%, #ffffff);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
}
.logo {
  width: 480rpx;
  margin-bottom: 96rpx;
}
.card {
  width: 100%;
  background: #ffffff;
  padding: 80rpx 48rpx;
  box-shadow: 0 24rpx 48rpx rgba(45, 140, 240, 0.12);
  display: flex;
  flex-direction: column;
  align-items: center;
}
.wechat-button {
  width: 100%;
  height: 96rpx;
  line-height: 96rpx;
  text-align: center;
  border-radius: 48rpx;
  font-size: 32rpx;
  font-weight: 600;
  color: #ffffff;
  background: linear-gradient(135deg, #05c160, #01934b);
}
.skip {
  margin-top: 32rpx;
  font-size: 28rpx;
  color: #9ca3af;
}
.agreement {
  margin-top: 48rpx;
  width: 100%;
}
.agreement__item {
  display: flex;
  align-items: center;
  gap: 16rpx;
}
.agreement__text {
  font-size: 26rpx;
  color: #6b7280;
}
</style>
