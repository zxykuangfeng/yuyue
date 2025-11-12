<template>
  <view class="page">
    <view class="notice-card">
      <view class="notice-title">预约须知</view>
      <text class="notice-text">
        请提前如实填写访客预约信息，确保个人身份信息准确无误。预约审核通过后，需携带有效身份证件入校配合安检。若计划变动，请及时取消或修改预约。
      </text>
    </view>
    <view class="form-card">
      <view class="form-title">预约信息填写</view>
      <uni-forms ref="applyForm" :modelValue="formData" :rules="rules" label-width="200rpx">
        <uni-forms-item label="所在校区" name="campus">
          <uni-data-select :localdata="campusOptions" v-model="formData.campus" placeholder="请选择校区" />
        </uni-forms-item>
        <uni-forms-item label="姓名" name="name">
          <uni-easyinput v-model="formData.name" placeholder="请输入姓名" />
        </uni-forms-item>
        <uni-forms-item label="身份证号" name="idNumber">
          <uni-easyinput v-model="formData.idNumber" placeholder="请输入身份证号" maxlength="18" />
        </uni-forms-item>
        <uni-forms-item label="进校类型" name="entryType">
          <uni-data-select :localdata="entryTypeOptions" v-model="formData.entryType" placeholder="请选择进校类型" />
        </uni-forms-item>
        <uni-forms-item label="进校时间" name="entryTime">
          <uni-datetime-picker v-model="formData.entryTime" type="datetime" placeholder="请选择进校时间" />
        </uni-forms-item>
        <uni-forms-item label="离校时间" name="leaveTime">
          <uni-datetime-picker v-model="formData.leaveTime" type="datetime" placeholder="请选择离校时间" />
        </uni-forms-item>
        <uni-forms-item label="工作单位" name="company">
          <uni-easyinput v-model="formData.company" placeholder="请输入工作单位" />
        </uni-forms-item>
        <uni-forms-item label="上传人脸" name="face">
          <uni-file-picker
            file-mediatype="image"
            :limit="1"
            mode="grid"
            return-type="object"
            @success="onFaceUpload"
            @delete="onFaceDelete"
          />
        </uni-forms-item>
        <uni-forms-item label="同行人员" name="companions">
          <uni-easyinput type="textarea" autoHeight v-model="formData.companions" placeholder="请输入同行人员信息（可选）" />
        </uni-forms-item>
      </uni-forms>
      <button class="primary-button" @click="submit">提交预约</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        campus: '',
        name: '',
        idNumber: '',
        entryType: '',
        entryTime: '',
        leaveTime: '',
        company: '',
        face: '',
        companions: ''
      },
      campusOptions: [
        { text: '通榆校区', value: 'tongyu' },
        { text: '希望大道校区', value: 'hope' }
      ],
      entryTypeOptions: [
        { text: '邀请入校', value: 'invite' },
        { text: '访客入校', value: 'visitor' }
      ],
      rules: {
        campus: { required: true, errorMessage: '请选择所在校区' },
        name: { required: true, errorMessage: '请输入姓名' },
        idNumber: { required: true, errorMessage: '请输入身份证号' },
        entryType: { required: true, errorMessage: '请选择进校类型' },
        entryTime: { required: true, errorMessage: '请选择进校时间' },
        leaveTime: { required: true, errorMessage: '请选择离校时间' },
        company: { required: true, errorMessage: '请输入工作单位' },
        face: { required: true, errorMessage: '请上传人脸照片' }
      }
    };
  },
  methods: {
    onFaceUpload(event) {
      const { tempFiles = [], tempFilePaths = [] } = event || {};
      const file = tempFiles[0] || {};
      this.formData.face = file.path || file.url || tempFilePaths[0] || '';
      this.$refs.applyForm && this.$refs.applyForm.setValue('face', this.formData.face);
    },
    onFaceDelete() {
      this.formData.face = '';
      this.$refs.applyForm && this.$refs.applyForm.setValue('face', '');
    },
    async submit() {
      try {
        await this.$refs.applyForm.validate();
      } catch (err) {
        return;
      }
      if (new Date(this.formData.leaveTime) <= new Date(this.formData.entryTime)) {
        uni.showToast({ title: '离校时间需晚于进校时间', icon: 'none' });
        return;
      }
      uni.showLoading({ title: '提交中' });
      setTimeout(() => {
        uni.hideLoading();
        uni.showToast({ title: '预约提交成功', icon: 'success' });
        setTimeout(() => {
          uni.navigateBack();
        }, 600);
      }, 800);
    }
  }
};
</script>

<style scoped>
.page {
  min-height: 100vh;
  padding: 48rpx 32rpx 80rpx;
  box-sizing: border-box;
  background: #f5f7fa;
}

.notice-card {
  background: #ffffff;
  border-radius: 24rpx;
  padding: 32rpx;
  margin-bottom: 40rpx;
  box-shadow: 0 24rpx 48rpx rgba(45, 140, 240, 0.08);
}

.notice-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1c2333;
  margin-bottom: 16rpx;
}

.notice-text {
  font-size: 30rpx;
  line-height: 48rpx;
  color: #4b5563;
}

.form-card {
  background: #ffffff;
  border-radius: 24rpx;
  padding: 40rpx 32rpx 48rpx;
  box-shadow: 0 24rpx 48rpx rgba(45, 140, 240, 0.12);
}

.form-title {
  font-size: 36rpx;
  font-weight: 600;
  color: #1c2333;
  margin-bottom: 32rpx;
}

.primary-button {
  margin-top: 48rpx;
  width: 100%;
  height: 96rpx;
  line-height: 96rpx;
  background: linear-gradient(135deg, #2d8cf0, #1c71d8);
  color: #ffffff;
  border-radius: 48rpx;
  font-size: 32rpx;
  font-weight: 600;
}
</style>