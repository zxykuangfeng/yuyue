<template>
  <view class="page">
    <view class="notice-card">
      <view class="notice-title">信息提示</view>
      <text class="notice-text">
        请完善您的基础信息，以便快速完成访客预约。请确保姓名、手机号和身份证号码真实有效，人脸照片将用于入校核验。
      </text>
    </view>
    <view class="form-card">
      <view class="form-title">我的信息</view>
      <uni-forms ref="profileForm" :modelValue="formData" :rules="rules" label-width="200rpx">
        <uni-forms-item label="姓名" name="name">
          <uni-easyinput v-model="formData.name" placeholder="请输入姓名" />
        </uni-forms-item>
        <uni-forms-item label="手机号" name="phone">
          <uni-easyinput v-model="formData.phone" placeholder="请输入手机号码" type="number" maxlength="11" />
        </uni-forms-item>
        <uni-forms-item label="身份证号" name="idNumber">
          <uni-easyinput v-model="formData.idNumber" placeholder="请输入身份证号" maxlength="18" />
        </uni-forms-item>
        <uni-forms-item label="人脸照片" name="face">
          <uni-file-picker
            file-mediatype="image"
            :limit="1"
            mode="grid"
            return-type="object"
            :value="faceList"
            @success="onFaceUpload"
            @delete="onFaceDelete"
          />
        </uni-forms-item>
      </uni-forms>
      <button class="primary-button" @click="handleSave">保存信息</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: '',
        phone: '',
        idNumber: '',
        face: ''
      },
      faceList: [],
      rules: {
        name: { required: true, errorMessage: '请输入姓名' },
        phone: {
          required: true,
          errorMessage: '请输入手机号码',
          validateFunction: (rule, value, data, callback) => {
            const phoneReg = /^1[3-9]\d{9}$/;
            if (!phoneReg.test(value)) {
              callback('请输入正确的手机号');
              return false;
            }
            callback();
            return true;
          }
        },
        idNumber: {
          required: true,
          errorMessage: '请输入身份证号',
          validateFunction: (rule, value, data, callback) => {
            const idReg = /^(\d{15}|\d{17}[\dXx])$/;
            if (!idReg.test(value)) {
              callback('请输入正确的身份证号');
              return false;
            }
            callback();
            return true;
          }
        },
        face: { required: true, errorMessage: '请上传人脸照片' }
      }
    };
  },
  onShow() {
    this.loadProfile();
  },
  methods: {
    loadProfile() {
      uni.getStorage({
        key: 'visitorProfile',
        success: ({ data }) => {
          const profile = data || {};
          const merged = {
            ...this.formData,
            ...profile
          };
          this.formData = merged;
          this.faceList = merged.face ? [{ url: merged.face }] : [];
          this.$nextTick(() => {
            if (this.$refs.profileForm) {
              ['name', 'phone', 'idNumber', 'face'].forEach((field) => {
                this.$refs.profileForm.setValue(field, merged[field] || '');
              });
            }
          });
        },
        fail: () => {
          this.faceList = [];
        }
      });
    },
    onFaceUpload(event) {
      const { tempFiles = [], tempFilePaths = [] } = event || {};
      const file = tempFiles[0] || {};
      this.formData.face = file.path || file.url || tempFilePaths[0] || '';
      this.faceList = this.formData.face ? [{ url: this.formData.face }] : [];
      this.$refs.profileForm && this.$refs.profileForm.setValue('face', this.formData.face);
    },
    onFaceDelete() {
      this.formData.face = '';
      this.faceList = [];
      this.$refs.profileForm && this.$refs.profileForm.setValue('face', '');
    },
    async handleSave() {
      try {
        await this.$refs.profileForm.validate();
      } catch (err) {
        return;
      }
      uni.setStorage({
        key: 'visitorProfile',
        data: this.formData,
        success: () => {
          uni.showToast({ title: '保存成功', icon: 'success' });
        }
      });
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