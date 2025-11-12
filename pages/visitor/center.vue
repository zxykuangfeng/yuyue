<template>
  <view class="page">
    <image class="banner" src="/static/logo.png" mode="widthFix" />
    <view class="menu-card">
      <view class="menu-title">快捷入口</view>
      <view class="menu-grid">
        <view class="menu-item" @click="goApply">
          <image class="menu-item__icon" src="/static/c1.png" mode="aspectFill" />
          <text class="menu-item__text">访客业务</text>
        </view>
        <view class="menu-item" @click="logout">
          <image class="menu-item__icon" src="/static/c2.png" mode="aspectFill" />
          <text class="menu-item__text">注销账号</text>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  methods: {
    goApply() {
      uni.navigateTo({ url: '/pages/visitor/apply' });
    },
    logout() {
      uni.showModal({
        title: '提示',
        content: '确认要注销当前账号吗？',
        confirmText: '确认',
        cancelText: '取消',
        success: ({ confirm }) => {
          if (confirm) {
            uni.showToast({ title: '已注销', icon: 'success' });
            setTimeout(() => {
              uni.reLaunch({ url: '/pages/index/index' });
            }, 500);
          }
        }
      });
    }
  }
};
</script>

<style scoped>
.page {
  min-height: 100vh;
  padding: 48rpx 32rpx 64rpx;
  box-sizing: border-box;
  background: linear-gradient(180deg, #ebf4ff, #f8fbff 45%, #ffffff);
}

.banner {
  width: 100%;
  border-radius: 24rpx;
  margin-bottom: 48rpx;
  background: #ffffff;
}

.menu-card {
  background: #ffffff;
  border-radius: 24rpx;
  padding: 40rpx 32rpx 16rpx;
  box-shadow: 0 24rpx 48rpx rgba(45, 140, 240, 0.12);
}

.menu-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1c2333;
  margin-bottom: 32rpx;
}

.menu-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 32rpx;
}

.menu-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 32rpx 24rpx;
  background: linear-gradient(135deg, rgba(45, 140, 240, 0.12), rgba(33, 150, 243, 0.08));
  border-radius: 24rpx;
  transition: transform 0.2s ease;
}

.menu-item:active {
  transform: scale(0.98);
}

.menu-item__icon {
  width: 96rpx;
  height: 96rpx;
  margin-bottom: 16rpx;
}

.menu-item__text {
  font-size: 30rpx;
  color: #1c2333;
  font-weight: 500;
}
</style>