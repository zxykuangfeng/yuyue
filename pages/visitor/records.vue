<template>
  <view class="page">
    <view class="notice">
      <view class="notice__title">预约提醒</view>
      <view class="notice__text">以下记录展示最近的访客预约信息，如需修改或取消请联系邀请人或校方工作人员。</view>
    </view>
    <view v-if="records.length" class="list">
      <view v-for="item in records" :key="item.id" class="card">
        <view class="card__header">
          <text class="card__title">{{ item.campus }} · {{ item.type }}</text>
          <text class="card__status" :class="'card__status--' + item.status">{{ statusLabel[item.status] }}</text>
        </view>
        <view class="card__row">
          <text class="card__label">访客姓名</text>
          <text class="card__value">{{ item.name }}</text>
        </view>
        <view class="card__row">
          <text class="card__label">进校时间</text>
          <text class="card__value">{{ item.enterTime }}</text>
        </view>
        <view class="card__row">
          <text class="card__label">离校时间</text>
          <text class="card__value">{{ item.leaveTime }}</text>
        </view>
        <view class="card__row">
          <text class="card__label">邀请人</text>
          <text class="card__value">{{ item.inviter }}</text>
        </view>
      </view>
    </view>
    <view v-else class="empty">
      <image class="empty__icon" src="/static/c8.png" mode="aspectFit" />
      <text class="empty__text">暂无预约记录</text>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      statusLabel: {
        pending: '待审核',
        approved: '已通过',
        rejected: '未通过'
      },
      records: [
        {
          id: 1,
          campus: '盐城校区',
          type: '访客入校',
          name: '张三',
          enterTime: '2023-09-12 09:00',
          leaveTime: '2023-09-12 18:00',
          inviter: '李老师',
          status: 'approved'
        },
        {
          id: 2,
          campus: '射阳校区',
          type: '邀请入校',
          name: '李四',
          enterTime: '2023-09-15 13:30',
          leaveTime: '2023-09-15 16:30',
          inviter: '王老师',
          status: 'pending'
        }
      ]
    };
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

.notice {
  background: #ffffff;
  border-radius: 24rpx;
  padding: 32rpx;
  box-shadow: 0 20rpx 40rpx rgba(45, 140, 240, 0.08);
  margin-bottom: 32rpx;
}

.notice__title {
  font-size: 32rpx;
  font-weight: 600;
  color: #1c2333;
  margin-bottom: 12rpx;
}

.notice__text {
  font-size: 26rpx;
  color: #6b7280;
  line-height: 1.6;
}

.list {
  display: flex;
  flex-direction: column;
  gap: 24rpx;
}

.card {
  background: #ffffff;
  border-radius: 24rpx;
  padding: 32rpx;
  box-shadow: 0 20rpx 40rpx rgba(45, 140, 240, 0.08);
}

.card__header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 24rpx;
}

.card__title {
  font-size: 30rpx;
  font-weight: 600;
  color: #1c2333;
}

.card__status {
  font-size: 26rpx;
  font-weight: 500;
}

.card__status--approved {
  color: #059669;
}

.card__status--pending {
  color: #f59e0b;
}

.card__status--rejected {
  color: #ef4444;
}

.card__row {
  display: flex;
  justify-content: space-between;
  font-size: 26rpx;
  color: #4b5563;
  margin-bottom: 16rpx;
}

.card__row:last-child {
  margin-bottom: 0;
}

.card__label {
  color: #9ca3af;
}

.card__value {
  color: #1f2937;
}

.empty {
  margin-top: 160rpx;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #9ca3af;
}

.empty__icon {
  width: 200rpx;
  height: 200rpx;
  margin-bottom: 24rpx;
}

.empty__text {
  font-size: 28rpx;
}
</style>