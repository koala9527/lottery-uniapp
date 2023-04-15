<template>
    <view class="container">
        <textarea v-model="feedbackContent" class="feedback-input" placeholder="请留下您的宝贵意见" maxlength="200"></textarea>
        <button type="primary" class="submit-btn" @tap="submitFeedback()">提交</button>
    </view>
</template>
  
<script>
export default {
    data() {
        return {
            feedbackContent: '',
        }
    },
    methods: {
        submitFeedback() {
            // TODO: 将feedbackContent提交到后台或其他处理逻辑
            if (this.feedbackContent == '') {
                uni.showToast({
                    title: '内容为空~',
                    icon: 'error',
                    duration: 2000
                })
                return '';
            }
            uni.request({
                url: 'https://*****.****.***/suggest',
                method: 'POST',
                data: {
                    content: this.feedbackContent
                },
                success: (res) => {
                    console.log('suggest成功:', res);
                },
                fail: (res) => {
                    console.log('suggest失败:', res);
                }

            });
            uni.showToast({
                title: '提交成功！',
                icon: 'success',
            })
            this.feedbackContent = ''
        },
    },
}
</script>
  
<style>
.container {
    padding: 20px;
}

.feedback-input {
    height: 300rpx;
    padding: 10rpx;
    font-size: 30rpx;
    line-height: 40rpx;
    border-radius: 10rpx;
    border: none;
    box-shadow: 0 0 5rpx rgba(0, 0, 0, .1);
    resize: none;
}

.submit-btn {
    margin-top: 30rpx;
    width: 100%;
    height: 90rpx;
    line-height: 90rpx;
    font-size: 38rpx;
    background-color: #f09883;
    color: #fff;
    border: none;
    border-radius: 10rpx;
    box-shadow: 0 0 5rpx rgba(0, 0, 0, .2);
    text-align: center;
}
</style>
  ``