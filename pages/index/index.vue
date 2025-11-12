<template>
	<view class="home">
	    <image class="home__banner" src="/static/logo.png" mode="widthFix" />
	    <view class="home__buttons">
	      <button class="primary-button" @click="goVisitor">访客申请</button>
	      <button class="primary-button" @click="goTeacher">在校师生</button>
		  
	    </view>
	    <view class="home__notice">
	      <view class="home__notice-title">预约须知</view>
	      <text class="home__notice-text">
	        欢迎使用盐城幼儿师范专科学院访客预约系统。访客需提前提交真实有效的预约信息，入校时请配合门岗核验身份。
	        在校师生如需协助访客预约，请提前与访客确认行程并做好信息审核。
	      </text>
	    </view>
	  </view>
</template>

<script>
export default {
  methods: {
    goVisitor() {
      uni.navigateTo({ url: '/pages/visitor/login' });
    },
    goTeacher() {
      uni.navigateTo({ url: '/pages/teacher/login' });
    }
  }
};
</script>
<style scoped>
.home {
  min-height: 100vh;
  padding: 48rpx 32rpx;
  background: #f5f7fa;
  box-sizing: border-box;
}

.home__banner {
  width: 100%;
  margin-bottom: 48rpx;
  background: #ffffff;
}

.home__buttons {
  display: flex;
  justify-content: space-between;
  gap: 32rpx;
  margin-bottom: 56rpx;
}

.primary-button {
  flex: 1;
  height: 96rpx;
  line-height: 96rpx;
  background: linear-gradient(135deg, #2d8cf0, #1c71d8);
  color: #ffffff;
  font-size: 32rpx;
  font-weight: 600;
  box-shadow: 0 16rpx 32rpx rgba(45, 140, 240, 0.2);
}

.primary-button:active {
  opacity: 0.9;
}

.home__notice {
  background: #ffffff;
  padding: 32rpx;
  box-shadow: 0 16rpx 40rpx rgba(31, 56, 88, 0.08);
}

.home__notice-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1c2333;
  margin-bottom: 16rpx;
}

.home__notice-text {
  font-size: 30rpx;
  line-height: 48rpx;
  color: #4b5563;
}
</style>
