<template>
  <view class="page">
    <image class="banner" src="/static/top.png" mode="widthFix" />
    <view class="card">
      <view class="card__title">在校师生登录</view>
      <view class="tabs">
        <view
          class="tab-item"
          :class="{ 'tab-item--active': activeTab === 'phone' }"
          @click="activeTab = 'phone'"
        >
          手机号验证码登录
        </view>
        <view
          class="tab-item"
          :class="{ 'tab-item--active': activeTab === 'account' }"
          @click="activeTab = 'account'"
        >
          账号密码登录
        </view>
      </view>

      <view v-if="activeTab === 'phone'" class="tab-panel">
        <uni-forms :modelValue="phoneForm" label-width="160rpx">
          <uni-forms-item label="手机号" name="phone">
            <uni-easyinput v-model="phoneForm.phone" placeholder="请输入手机号" type="number" maxlength="11" />
          </uni-forms-item>
          <uni-forms-item label="验证码" name="code">
            <view class="code-row">
              <uni-easyinput class="code-row__input" v-model="phoneForm.code" placeholder="请输入验证码" />
              <button class="code-row__button" @click="sendPhoneCode" :disabled="countdown > 0">
                {{ countdown > 0 ? countdown + 's' : '获取验证码' }}
              </button>
            </view>
          </uni-forms-item>
        </uni-forms>
        <button class="primary-button" @click="submitPhone">登录</button>
      </view>

      <view v-else class="tab-panel">
        <uni-forms :modelValue="accountForm" label-width="160rpx">
          <uni-forms-item label="账号" name="username">
            <uni-easyinput v-model="accountForm.username" placeholder="请输入账号" />
          </uni-forms-item>
          <uni-forms-item label="密码" name="password">
            <uni-easyinput v-model="accountForm.password" type="password" placeholder="请输入密码" />
          </uni-forms-item>
        </uni-forms>
        <button class="primary-button" @click="submitAccount">登录</button>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      activeTab: 'phone',
      phoneForm: {
        phone: '',
        code: ''
      },
      accountForm: {
        username: '',
        password: ''
      },
      countdown: 0,
      timer: null
    };
  },
  methods: {
    sendPhoneCode() {
      if (!this.phoneForm.phone) {
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
    submitPhone() {
      if (!this.phoneForm.phone || !this.phoneForm.code) {
        uni.showToast({ title: '请完善登录信息', icon: 'none' });
        return;
      }
      uni.showToast({ title: '登录成功', icon: 'success' });
    },
    submitAccount() {
      if (!this.accountForm.username || !this.accountForm.password) {
        uni.showToast({ title: '请输入账号和密码', icon: 'none' });
        return;
      }
      uni.showToast({ title: '登录成功', icon: 'success' });
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

.tabs {
  display: flex;
  background: #f3f4f6;
  padding: 8rpx;
  margin-bottom: 32rpx;
}

.tab-item {
  flex: 1;
  text-align: center;
  height: 80rpx;
  line-height: 80rpx;
  font-size: 28rpx;
  color: #4b5563;
}

.tab-item--active {
  background: linear-gradient(135deg, #2d8cf0, #1c71d8);
  color: #ffffff;
  font-weight: 600;
  box-shadow: 0 12rpx 24rpx rgba(45, 140, 240, 0.25);
}

.tab-panel {
  animation: fadeIn 0.2s ease;
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

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(16rpx);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>