<template>
  <view class="page">
    <view class="card">
      <view class="card__title">专属邀请码</view>
      <view class="card__subtitle">邀请访客扫描二维码或使用链接填写预约信息</view>
      <view class="qr-box">
        <image class="qr-box__image" src="/static/c6.png" mode="aspectFill" />
        <text class="qr-box__label">扫码填写预约信息</text>
      </view>
      <view class="link-box">
        <view class="link-box__row">
          <text class="link-box__label">邀请链接</text>
          <button class="copy-button" @click="copyLink">复制</button>
        </view>
        <text class="link-box__value">{{ inviteLink }}</text>
      </view>
      <view class="tips">
        <view class="tips__title">使用说明</view>
        <view class="tips__item">1. 访客扫码后将进入预约填写页面，系统会自动带出您的信息为被访人。</view>
        <view class="tips__item">2. 可将链接分享给访客，访客在浏览器中打开同样可填写预约。</view>
        <view class="tips__item">3. 访客提交后，您可在“我的预约”中查看最新记录。</view>
      </view>
    </view>
    <button class="preview-button" @click="previewForm">预览访客填写页面</button>
  </view>
</template>

<script>
export default {
  data() {
    return {
      inviteLink: '',
      profile: {
        name: '在校师生'
      }
    };
  },
  onShow() {
    const profile = uni.getStorageSync('teacherProfile');
    if (profile && profile.name) {
      this.profile = profile;
    }
    this.inviteLink = this.buildInviteLink();
  },
  methods: {
    buildInviteLink() {
      const host = encodeURIComponent(this.profile.name || '在校师生');
      return `/pages/visitor/apply?host=${host}`;
    },
    copyLink() {
      if (!this.inviteLink) {
        uni.showToast({ title: '暂无可复制的链接', icon: 'none' });
        return;
      }
      uni.setClipboardData({
        data: this.inviteLink,
        success: () => {
          uni.showToast({ title: '链接已复制', icon: 'success' });
        }
      });
    },
    previewForm() {
      const host = encodeURIComponent(this.profile.name || '在校师生');
      uni.navigateTo({ url: `/pages/visitor/apply?host=${host}` });
    }
  }
};
</script>

<style scoped>
.page {
  min-height: 100vh;
  padding: 48rpx 32rpx 80rpx;
  box-sizing: border-box;
  background: linear-gradient(180deg, #ebf4ff, #f8fbff 45%, #ffffff);
  display: flex;
  flex-direction: column;
}

.card {
  background: #ffffff;
  border-radius: 24rpx;
  padding: 40rpx 32rpx;
  box-shadow: 0 24rpx 48rpx rgba(45, 140, 240, 0.12);
}

.card__title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1c2333;
  margin-bottom: 12rpx;
}

.card__subtitle {
  font-size: 28rpx;
  color: #6b7280;
  margin-bottom: 32rpx;
}

.qr-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, rgba(45, 140, 240, 0.1), rgba(33, 150, 243, 0.06));
  border-radius: 24rpx;
  padding: 40rpx 24rpx;
  margin-bottom: 32rpx;
}

.qr-box__image {
  width: 280rpx;
  height: 280rpx;
  margin-bottom: 16rpx;
}

.qr-box__label {
  font-size: 28rpx;
  color: #1c2333;
}

.link-box {
  background: #f3f4f6;
  border-radius: 24rpx;
  padding: 24rpx;
  margin-bottom: 32rpx;
}

.link-box__row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16rpx;
}

.link-box__label {
  font-size: 28rpx;
  color: #4b5563;
}

.link-box__value {
  font-size: 26rpx;
  color: #1f2937;
  word-break: break-all;
}

.copy-button {
  padding: 12rpx 32rpx;
  border-radius: 32rpx;
  background: linear-gradient(135deg, #2d8cf0, #1c71d8);
  color: #ffffff;
  font-size: 26rpx;
}

.tips {
  display: flex;
  flex-direction: column;
  gap: 12rpx;
  color: #4b5563;
  font-size: 26rpx;
}

.tips__title {
  font-size: 30rpx;
  font-weight: 600;
  color: #1c2333;
}

.tips__item {
  line-height: 1.6;
}

.preview-button {
  margin-top: 40rpx;
  height: 96rpx;
  line-height: 96rpx;
  background: linear-gradient(135deg, #2d8cf0, #1c71d8);
  color: #ffffff;
  border-radius: 48rpx;
  font-size: 32rpx;
  font-weight: 600;
}
</style>