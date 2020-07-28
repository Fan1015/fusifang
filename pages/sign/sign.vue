<template>
	<view class="editPassword">
		<view class="editPassword-top">
			<view class="editPassword-top-h1">Hi，请输入以下信息</view>
			<view class="editPassword-top-h2">注意信息填写正确</view>
		</view>
		<view class="editPassword-bottom">
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">手机号</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="number"
					@input="telChange"
					value=""
					placeholder="请输入手机号"
				/>
			</view>
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">密码</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="password"
					@input="newpasswordChange"
					value=""
					placeholder="请输入密码"
				/>
			</view>
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">确认密码</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="password"
					@input="confirmpasswordChange"
					value=""
					placeholder="请再次输入密码"
				/>
			</view>
		</view>
		<view class="editPassword-button"><button type="primary" @click="phoneBtn">保存</button></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			newPassword: '', //密码
			confirmpassword: '', //确认密码
			phone: '', //手机号
			userid: '' ,//用户id
			loginId:''//邀请码
		};
	},
	onLoad(option) {
		console.log(option);
		this.loginId = option.loginId
		this.editId = option.editId;
		let userId = uni.getStorageSync('userId');
		this.userid = userId;
	},
	methods: {
		// 手机号
		telChange(tel) {
			this.phone = tel.detail.value;
		},
		// 密码
		newpasswordChange(newpass) {
			this.newPassword = newpass.detail.value;
		},

		// 确认密码
		confirmpasswordChange(confirm) {
			this.confirmpassword = confirm.detail.value;
		},

		// 保存按钮
		phoneBtn(newbtn) {
			let that = this;
			console.log(newbtn);
			console.log(that.newPassword); //密码
			console.log(that.phone); //手机号
			console.log(that.confirmpassword); //确认密码
			if (that.phone) {
				if (that.newPassword) {
					if (that.confirmpassword) {
						uni.request({
							url: app.globalData.api + 'Login/webRegister',
							method: 'post',
							header: {
								'content-type': 'application/x-www-form-urlencoded'
							},
							data: {
								tel: that.phone,
								loginPwd: that.newPassword,
								loginRePwd: that.confirmpassword,
								loginId:that.loginId
							},
							success(loginres) {
								console.log(loginres);
								if(loginres.data.status == 1){
									uni.navigateTo({
										url:'../login/login'
									})
								}else{
									uni.showToast({
										title: loginres.data.msg,
										icon: 'none'
									});
								}
							}
						});
					} else {
						uni.showToast({
							title: '请再次确认密码',
							icon: 'none'
						});
					}
				} else {
					uni.showToast({
						title: '请输入密码',
						icon: 'none'
					});
				}
			} else {
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
.editPassword {
	padding: 32upx;
	&-top {
		&-h1 {
			font-size: 46upx;
			color: rgba(0, 0, 0, 1);
			font-weight: 400;
			line-height: 65upx;
			letter-spacing: 3upx;
		}
		&-h2 {
			font-size: 24upx;
			color: rgba(102, 182, 112, 1);
			font-weight: 400;
			line-height: 33upx;
			letter-spacing: 2upx;
			margin-top: 20upx;
		}
	}
	&-bottom {
		&-item {
			margin-top: 60upx;
			border-bottom: 2upx solid rgba(248, 248, 249, 1);
			&-text {
				font-size: 28upx;
				color: rgba(51, 51, 51, 1);
				font-weight: 400;
				line-height: 40upx;
				letter-spacing: 2upx;
			}
			&-input {
				font-size: 34upx;
				font-weight: 400;
				color: rgba(51, 51, 51, 1);
				line-height: 48upx;
				letter-spacing: 2upx;
				padding: 36upx 0 29upx;
			}
			&-place {
				font-size: 34upx;
				font-weight: 400;
				color: rgba(187, 187, 187, 1);
				line-height: 48upx;
				letter-spacing: 2upx;
			}
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
}
</style>
