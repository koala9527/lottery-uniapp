<template>
	<view class="container">
		<view class="header">
			<img :src="userInfo.avatar" alt="头像" class="avatar" />
			<p class="nickname">{{ userInfo.nickname }}</p>
		</view>
		<view class="section">

			<view class="line"></view>
			<view class="item" @click="jumpSuggest">
				<!-- <img src="/static/images/history.png" alt="意见反馈" /> -->
				<span>意见反馈</span><span>></span>
			</view>
			<!-- <view class="line"></view>
			<view class="item">
				<img src="/static/images/about.png" alt="关于小程序" />
				<span>关于小程序</span>
			</view> -->
			<view class="line"></view>
			<view class="item" @click="jumpHelp">
				<!-- <img src="/static/images/suggestion.png" alt="帮助信息" /> -->
				<span>帮助信息</span><span>></span>
			</view>
		</view>
		<view class="tell">
			彩票玩法支持：<br>支持体彩超级大乐透和福彩双色球，支持单式、复式、胆拖玩法。
		</view>
	</view>
</template>
  
<script>
export default {
	data() {
		return {
			authCode: '', // 鉴权code
			userId: '',
			userInfo: {}, // mock数据
		};
	},
	mounted() {
		this.getUserInfo();
		this.onGetAuthCode();
	},
	methods: {
		getUserInfo() {
			// 这里可以调用接口获取用户信息，这里为了演示使用了mock数据
			this.userInfo = {
				avatar: "/static/avator.png",
				nickname: "金钱豹",
			};
		},
		jumpSuggest() {
			uni.navigateTo({
				url: '/pages/home/suggest',
				// 其他可选参数，如 query、animation 等等
			});
		},
		jumpHelp() {
			uni.navigateTo({
				url: '/pages/home/help',
				// 其他可选参数，如 query、animation 等等
			});
		},
		onGetAuthCode() {
			let userId = uni.getStorageSync('userId');
			console.log(userId)
			if (userId == undefined || userId == '') {
				uni.getAuthCode({
					scopes: 'auth_base',
					success: (res) => {
						console.log('authCode:', res.authCode)
						if (res.authCode) {
							this.authCode = res.authCode;
							this.requestAccessToken();

						}
					},
					fail: (res) => {
						console.log(res);
					},
				});
			} else {
				this.userId = userId;
			}
		},

		requestAccessToken() {
			uni.request({
				url: 'https://*****.****.***/get_user_id',
				method: 'POST',
				data: {
					code: this.authCode
				},
				success: (res) => {
					console.log(res.data.data)
					if (res.data.status == true) {
						// const accessToken = res.data.accessToken;
						uni.setStorageSync('userId', res.data.data.user_id);
						// TODO: 将accessToken和userId保存在本地或发送至服务器端。
					} else {
						console.log(res.data.msg);
					}
				},
				fail: (res) => {
					console.log('获取userId失败:', res);
				}
			});
		}
	}
}
</script>
  
<style scoped>
.container {
	width: 100%;
	height: 100%;
	overflow: hidden;
}

.header {
	display: flex;
	align-items: center;
	padding: 40rpx;
	background-color: #f09883;
	color: #fff;
	border-radius: 20rpx;
	margin: 20rpx;
}

.avatar {

	width: 150rpx;
	height: 150rpx;
	border-radius: 50%;
	margin-right: 20rpx;
}

.nickname {
	font-size: 18px;
	font-weight: bold;
	color: #000;
}

.section {
	margin-top: 20px;
	background-color: #f09883;
	margin: 20rpx;

}

.line {
	height: 10rpx;
	background-color: #e4e4e4;
}

.item {
	display: flex;
	align-items: center;
	justify-content: space-between;
	padding: 15px 20px;
	background-color: #f09883;
	border-radius: 50rpx;
}

.item img {
	width: 24px;
	height: 24px;
	margin-right: 10px;

}

.item span {
	font-size: 28rpx;
	font-weight: 600;
	text-align: left;
}


.item:last-child {
	margin-bottom: 20px;
}

.tell {
	margin: 20rpx;
	align-items: center;
	justify-content: space-between;
	padding: 15px 20px;
	background-color: #f0aa99;
}
</style>
  