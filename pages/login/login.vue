<template>
	<view class="login">
		<view class="myPurse-top">
			<view class="myPurse-top-h1" @click="goPagePrve"></view>
			<view class="myPurse-top-h2">用户登录</view>
		</view>
		<view class="login-item">
			<input type="number" value="" @input="userNumChange" placeholder="请输入11位手机号码" placeholder-class="login-item-place" class="login-item-input" />
			<input type="password" value="" @input="userpasswordChange" placeholder="请输入密码" placeholder-class="login-item-place" class="login-item-input" />
		</view>
		<view class="login-memory">
			<!-- <navigator url="../editPassword/editPassword?editId=3" class="login-memory-text">忘记密码</navigator> -->
			<!-- <navigator url="../sign/sign" class="login-memory-text">注册账号</navigator> -->
		</view>
		<view class="login-button"><button type="primary" @click="loginChange">登录</button></view>
		<!-- <view class="login-privacy">
			登录表示已阅读并同意
			<navigator url="">隐私政策</navigator>
			和
			<navigator url="">未成年人保护规则</navigator>
		</view> -->
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			userNumber: '', //账号（手机号）
			userpassword: '' //密码
		};
	},
	methods: {
		// 账号
		userNumChange(usernum) {
			this.userNumber = usernum.detail.value;
		},
		// 密码
		userpasswordChange(userpass) {
			this.userpassword = userpass.detail.value;
		},
		// 返回
		goPagePrve() {
			uni.switchTab({
				url: '../index/index'
			});
		},
		// 登录
		loginChange(loginbtn) {
			let that = this;
			console.log(loginbtn);
			if(this.userNumber){
				if(this.userpassword){
					uni.request({
						url: app.globalData.api + 'Login/webLogin',
						method: 'post',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						data: {
							tel: this.userNumber,
							loginPwd: this.userpassword
						},
						success(loginres) {
							console.log(loginres);
							if (loginres.data.status == 1) {
								uni.setStorageSync('userId', loginres.data.data.userId);
								uni.switchTab({
									url: '../member/member'
								});
							} else {
								uni.showToast({
									title: loginres.data.msg,
									icon: 'none'
								});
							}
						}
					});
				}else{
					uni.showToast({
						title: '请输入密码',
						icon: 'none'
					});
				}
			}else{
				uni.showToast({
					title: '请输入手机号',
					icon: 'none'
				});
			}
		}
	}
};
</script>

<style lang="scss" scoped>
page {
	background: rgba(255, 255, 255, 1);
}
.myPurse-top {
	position: fixed;
	left: 0;
	top: 0;
	width: 100%;
	height: 44px;
	height: calc(44px + constant(safe-area-inset-top));
	height: calc(44px + env(safe-area-inset-top));
	padding: 7px 3px;
	padding-top: calc(7px + constant(safe-area-inset-top));
	padding-top: calc(7px + env(safe-area-inset-top));
	display: -webkit-box;
	display: -webkit-flex;
	display: flex;
	align-items: center;
	justify-content: center;
	overflow: hidden;
	box-sizing: border-box;
	z-index: 998;
	-webkit-transition-property: all;
	transition-property: all;
	background: rgba(255, 255, 255, 1);
	&-h1 {
		position: relative;
		height: calc(44px + constant(safe-area-inset-top));
		height: calc(44px + env(safe-area-inset-top));
	}
	&-h1::before {
		content: '';
		width: 20upx;
		height: 20upx;
		border-top: 2upx solid rgba(0, 0, 0, 1);
		border-right: 2upx solid rgba(0, 0, 0, 1);
		transform: rotate(225deg);
		display: block;
		position: absolute;
		left: 32upx;
		top: 32upx;
	}
	&-h2 {
		width: 100%;
		font-size: 32upx;
		color: rgba(0, 0, 0, 1);
		line-height: 60upx;
		text-align: center;
		background-color: rgba(255, 255, 255, 1);
		font-weight: 700;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
	}
}
.login {
	padding: 0 32upx;
	&-item {
		margin-top: 202upx;
		&-input {
			font-size: 32upx;
			color: rgba(51, 51, 51, 1);
			font-weight: 500;
			line-height: 45upx;
			border-bottom: 2upx solid rgba(246, 246, 246, 1);
			padding: 0 0 20upx 0;
			margin-bottom: 74upx;
		}
		&-place {
			font-size: 32upx;
			color: rgba(200, 203, 207, 1);
			font-weight: 500;
			line-height: 45upx;
		}
	}
	&-memory {
		display: flex;
		justify-content: space-between;
		align-items: center;
		&-text {
			font-size: 24upx;
			color: rgba(52, 168, 255, 1);
			font-weight: 400;
			line-height: 33upx;
		}
	}

	&-button {
		margin-top: 100upx;
		button {
			background: rgba(55, 147, 223, 1);
			width: 670upx;
			height: 94upx;
		}
	}
	// &-privacy {
	// 	font-size: 26upx;
	// 	color: rgba(96, 96, 96, 1);
	// 	font-weight: 400;
	// 	line-height: 37upx;
	// 	display: flex;
	// 	justify-content: center;
	// 	align-items: center;
	// 	position: absolute;
	// 	bottom: 108upx;
	// 	left: 76upx;
	// 	> navigator {
	// 		color: #42b6ff;
	// 	}
	// }
}
</style>
