<template>
	<view class="editPassword">
		<view class="editPassword-top">
			<view class="editPassword-top-h1">Hi，请输入以下信息</view>
			<view class="editPassword-top-h2">注意信息填写正确</view>
		</view>
		<view class="editPassword-bottom">
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">新密码</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="password"
					@input="newpasswordChange"
					value=""
					placeholder="请输入新密码"
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
					placeholder="请再次输入新密码"
				/>
			</view>
		</view>
		<view class="editPassword-button" v-if="editId == 1"><button type="primary" @click="newpasswordBtn">保存</button></view>
		<view class="editPassword-button" v-if="editId == 2"><button type="primary" @click="secondpasswordBtn">保存</button></view>
		<view class="editPassword-button" v-if="editId == 3"><button type="primary" @click="oblivionBtn">保存</button></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			newPassword: '', //新密码
			confirmpassword: '', //确认密码
			wechat: '', //微信号
			editId: '', //修改密码ID，1为修改登录密码，2为修改二级密码,3为忘记密码
			userId: '' //用户id
		};
	},
	onLoad(option) {
		console.log(option);
		this.editId = option.editId;
		this.editKey = option.key;
		let userId = uni.getStorageSync('userId');
		this.userId = userId;
		console.log(this.userId);
	},
	methods: {
		// 新密码
		newpasswordChange(newpass) {
			this.newPassword = newpass.detail.value;
		},

		// 确认密码
		confirmpasswordChange(confirm) {
			this.confirmpassword = confirm.detail.value;
		},

		// 微信号
		wechatChange(wcc) {
			this.wechat = wcc.detail.value;
		},

		// 修改登录密码按钮
		newpasswordBtn(newbtn) {
			console.log(newbtn);
			console.log(this.editId);
			let that = this;
			uni.request({
				url: app.globalData.api + 'user/editPwd',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					userId: that.userId,
					type: that.editId,
					pwd: that.newPassword,
					rePwd: that.confirmpassword
				},
				success(editpwd) {
					console.log(editpwd);
					if (that.newPassword || that.confirmpassword) {
						if (editpwd.data.status == 1) {
							console.log(94);
							uni.showLoading({
								title: '提交中...'
							});
							setTimeout(function() {
								uni.hideLoading();
								uni.navigateBack({
									delta: 1
								});
							}, 300);
						} else {
							uni.showToast({
								title: editpwd.data.msg,
								icon: 'none'
							});
						}
					} else {
						uni.showToast({
							title: '请输入密码',
							icon: 'none'
						});
					}
				}
			});
		},

		// 修改二级密码保存
		secondpasswordBtn(secondbtn) {
			console.log(secondbtn);
			console.log(this.editId);
			let that = this;
			if (that.newPassword) {
				if (that.confirmpassword) {
					uni.request({
						url: app.globalData.api + 'user/editPwd',
						method: 'post',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						data: {
							userId: that.userId,
							type: that.editId,
							pwd: that.newPassword,
							rePwd: that.confirmpassword
						},
						success(editpwd) {
							console.log(editpwd);
							if (editpwd.data.status == 1) {
								uni.showLoading({
									title: '提交中...'
								});
								setTimeout(function() {
									uni.hideLoading();
									uni.navigateBack({
										delta: 1
									});
								}, 300);
							} else {
								uni.showToast({
									title: editpwd.data.msg,
									icon: 'none'
								});
							}
						}
					});
				} else {
					uni.showToast({
						title: '请确认密码',
						icon: 'none'
					});
				}
			} else {
				uni.showToast({
					title: '请输入密码',
					icon: 'none'
				});
			}
		},

		// 忘记密码
		oblivionBtn() {
			let that = this;
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
