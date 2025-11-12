<template>
  <view class="page">
    <image class="banner" src="/static/logo.png" mode="widthFix" />
    <view class="card">
      <view class="card__title">访客登录</view>
      <uni-forms ref="visitorForm" :modelValue="formData" label-width="160rpx">
        <uni-forms-item label="手机号" name="phone">
          <uni-easyinput v-model="formData.phone" placeholder="请输入手机号" type="number" maxlength="11" />
        </uni-forms-item>
        <uni-forms-item label="验证码" name="code">
          <view class="code-row">
            <uni-easyinput class="code-row__input" v-model="formData.code" placeholder="请输入验证码" />
            <button class="code-row__button" @click="sendCode" :disabled="countdown > 0">
              {{ countdown > 0 ? countdown + 's' : '获取验证码' }}
            </button>
          </view>
        </uni-forms-item>
      </uni-forms>
      <button class="primary-button" @click="submit">登录</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        phone: '',
        code: ''
      },
      countdown: 0,
      timer: null
    };
  },
  methods: {
    sendCode() {
      if (!this.formData.phone) {
        uni.showToast({ title: '请输入手机号', icon: 'none' });
        return;
      }
      uni.showToast({ title: '验证码已发送', icon: 'success' });
      this.startCountdown();
    },
    startCountdown() {
      this.countdown = 60;
      this.clearTimer();
      this.timer = setInterval(() => {
        if (this.countdown > 0) {
          this.countdown -= 1;
        } else {
          this.clearTimer();
        }
      }, 1000);
    },
    clearTimer() {
      if (this.timer) {
        clearInterval(this.timer);
        this.timer = null;
      }
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
    }
  },
  onUnload() {
    this.clearTimer();
  }
};
</script>

<style scoped>
.page {
  min-height: 100vh;
  padding: 48rpx 32rpx;
  box-sizing: border-box;
  background: linear-gradient(180deg, #ebf4ff, #f8fbff 45%, #ffffff);
}

.banner {
  width: 100%;
  margin-bottom: 48rpx;
  background: #ffffff;
}

.card {
  background: #ffffff;
  padding: 48rpx 32rpx;
  box-shadow: 0 24rpx 48rpx rgba(45, 140, 240, 0.12);
}

.card__title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1c2333;
  margin-bottom: 32rpx;
}

.code-row {
  display: flex;
  align-items: center;
  gap: 16rpx;
}

.code-row__input {
  flex: 1;
}

.code-row__button {
  padding: 0 24rpx;
  height: 80rpx;
  line-height: 80rpx;
  background: linear-gradient(135deg, #2d8cf0, #1c71d8);
  color: #ffffff;
  font-size: 28rpx;
}

.code-row__button:disabled {
  background: #a5b4fc;
  color: #ffffff;
}

.primary-button {
  margin-top: 48rpx;
  width: 100%;
  height: 96rpx;
  line-height: 96rpx;
  background: linear-gradient(135deg, #2d8cf0, #1c71d8);
  color: #ffffff;
  font-size: 32rpx;
  font-weight: 600;
}
</style>