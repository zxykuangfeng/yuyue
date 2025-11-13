<template>
  <view class="page">
    <image class="banner" src="/static/top.png" mode="widthFix" />
    <view class="menu-card">
      <view class="menu-title">快捷入口</view>
      <view class="menu-grid">
        <view class="menu-item" @click="goApply">
          <image class="menu-item__icon" src="/static/fangke.png" mode="aspectFill" />
          <text class="menu-item__text">访客业务</text>
        </view>
        <view class="menu-item" @click="goRecords">
          <image class="menu-item__icon" src="/static/yuyue.png" mode="aspectFill" />
          <text class="menu-item__text">预约记录</text>
        </view>
		 <view class="menu-item" @click="goProfile">
		          <image class="menu-item__icon" src="/static/user.png" mode="aspectFill" />
		          <text class="menu-item__text">我的信息</text>
		        </view>
        <view class="menu-item" @click="logout">
          <image class="menu-item__icon" src="/static/logoff.png" mode="aspectFill" />
          <text class="menu-item__text">注销账号</text>
        </view>
      </view>
    </view>
	
	<view class="home__footer">
		  			<image class="footer-logo" src="/static/logo.png" mode="widthFix"></image>
		  			<text class="footer-text">盐城幼儿师范高等专科学校</text>
		  		</view>
  </view>
</template>

<script>
export default {
  methods: {
    goApply() {
      uni.navigateTo({ url: '/pages/visitor/apply' });
    },
    goRecords() {
      uni.navigateTo({ url: '/pages/visitor/records' });
    },
	 goProfile() {
	      uni.navigateTo({ url: '/pages/visitor/profile' });
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
    display: flex;         /* 必须 */
    flex-direction: column;/* 必须，纵向布局 */
}

.banner {
  width: 100%;
  margin-bottom: 48rpx;
  background: #ffffff;
}

.menu-card {
  background: #ffffff;
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
  transition: transform 0.2s ease;
}

.menu-item:active {
  transform: scale(0.98);
}

.menu-item__icon {
  width: 56rpx;
  height: 56rpx;
  margin-bottom: 16rpx;
}

.menu-item__text {
  font-size: 30rpx;
  color: #1c2333;
  font-weight: 500;
}
.home__footer {
  margin-top: auto;
  padding: 40rpx 0;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 20rpx;
}

.footer-logo {
  width: 60rpx;
  height: 60rpx;
}

.footer-text {
  font-size: 30rpx;
  color: #1c71d8;
  font-weight: 600;
}
</style>