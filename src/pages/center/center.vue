<template>
	<view class="container">
		<button type="action-button" @click="chooseImage" class="input_pic">
			拍照或者相册选择彩票
		</button>
		<view class="show_pic">
			<button type="action-button" @click="show" class="input_pic">
				查看原图
			</button>
		</view>
		<view v-show="maskVisible" class="mask" @click="hideMask">
			<image :src="photoUrl"></image>
		</view>
		<view class="text-box">
			<text class="result" ref="myInput">{{ text }}</text>
		</view>
		<view class="power">
			<button type="action-button" @click="copy" class="power_btn">
				复制文本
			</button>
			<button  @click="share" class="power_btn">
				分享给朋友
			</button>
		</view>
	</view>
</template>
  
<script>
export default {

	data() {
		return {
			images: [],
			photoUrl: '',
			serverUrl: 'https://*****.****.***/predict',
			maskVisible: false, // 是否显示进度条
			text: '识别结果^_^',
			authCode: '',
		};
	},
	onload() {
		my.getAuthCode({
			scopes: 'auth_base',
			success: (res) => {
				this.authCode = res.authCode;
			},
		});
		my.onShareAppMessage(() => ({

			title: '发现了一个非常好用的彩票识别工具',
			path: '/pages/center/center',
			success: this.onShareSuccess, // 分享成功时的回调函数
			fail: this.onShareFail, // 分享失败时的回调函数

		}));
	},
	methods: {
		copy() {
			uni.showToast({
				title: '暂不支持',
			});
		},
		show() {
			if (this.photoUrl !== '') {
				this.maskVisible = true;
				uni.showToast({
					icon: 'none',
					duration: 0,
				});
			} else {
				uni.showToast({
					title: '未上传',
				});
			}
		},
		hideMask() {
			this.maskVisible = false;
			uni.hideToast();
		},

		share() {
			my.showSharePanel();
		},

		onShareSuccess() {
			console.log('分享成功');
		},
		onShareFail() {
			console.log('分享失败');
		},
		chooseImage() {
			uni.chooseImage({
				count: 1,
				success: (res) => {
					const tempFilePaths = res.tempFilePaths;
					console.log("选择成功")
					this.photoUrl = tempFilePaths[0]
					// 上传图片到服务器
					this.uploadFile(tempFilePaths[0]);
				},
				fail: err => {
					console.log("选择失败", err)
				}, // Function，选择失败后的回调函数
				complete: () => {
					console.log("选择完成")
				} // Function，无论选择成功或失败都会执行的回调函数
			});
		},
		uploadFile(filePath) {
			console.log([this.serverUrl])
			this.showProgress = true
			uni.uploadFile({
				url: this.serverUrl,
				filePath: filePath,
				name: 'file',
				success: (res) => {
					console.log('upload success', res);

					// 从mock服务器获取响应
					// this.getResponseFromMockServer();
					try {
						const obj = JSON.parse(res.data);
						if (obj.code == 200) {
							this.text = obj.data
						}
					} catch (e) {
						console.error('JSON转换异常', e);
					}
				},
				fail: (err) => {
					console.log('upload fail:');
					console.log(err)
					// 处理上传失败情况
					// ...
				},
				onProgressUpdate: (progress) => {
					console.log(progress)
					this.uploadProgress = progress.progress
					//   this.percentage = Math.round(progressEvent.loaded * 100 / progressEvent.total)
					if (this.progress < 50) {
						this.status = '正在分析图片'
					} else if (this.progress < 90) {
						this.status = '正在查询当前大奖结果'
					} else {
						this.status = '正在分析中奖结果'
					}
				},
				complete: () => {
					console.log('upload complete');
					// this.finish()
				},
			});
		}
	},
};
</script>
<style>
.container {
	width: 100%;
	position: relative;
	overflow: hidden;
}

.input_pic {
	background-color: #f09883;
	color: #fff;
	padding: 10rpx 20rpx;
	border-radius: 25rpx;
	font-size: 26rpx;
	font-weight: bold;
	text-align: center;
	display: flex;
	justify-content: center;
	align-items: center;
	margin: 30rpx 20rpx 20rpx 20rpx;
}

.input_pic:hover {
	background-color: #f08383;
}

.text-box {
	padding: 10rpx 20rpx;
	border-radius: 15px;
	border: 2rpx solid #f09883;
	padding: 10rpx;
	display: block;
	height: 50vh;
	margin: 10rpx 20rpx;
}

.result {
	font-size: 16px;
}

.mask {
	position: fixed;
	z-index: 999;
	top: 0;
	left: 0;
	width: 100%;
	height: 100vh;
	background-color: rgba(0, 0, 0, 0.7);
	display: flex;
	justify-content: center;
	align-items: center;
}

.mask image {
	max-width: 100%;
	max-height: 100%;
}

.power {
	display: flex;
	justify-content: center;
	align-items: center;
}

.power_btn {
	background-color: #f09883;
	color: #fff;
	border-radius: 25rpx;
	font-size: 26rpx;
	width: 300rpx;
	font-weight: bold;
	text-align: center;
	align-items: center;
	margin: 30rpx 20rpx 20rpx 20rpx;
}
</style>