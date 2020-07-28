<template>
	<view class="editPassword">
		<view class="editPassword-top">
			<view class="editPassword-top-h1">Hi，请输入以下信息</view>
			<view class="editPassword-top-h2">注意信息填写正确</view>
		</view>
		<view class="editPassword-bottom">
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">微信号</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="text"
					@input="wechatChange"
					value=""
					placeholder="请输入微信号"
				/>
			</view>
		</view>
		<view class="editPassword-button"><button type="primary" @click="editWechatBtn">保存</button></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			wechat: '', //微信号
			userId: '', //用户id
			editKey: '' //修改信息对应键值
		};
	},
	onLoad(option) {
		console.log(option);
		this.editKey = option.key;
		let userId = uni.getStorageSync('userId');
		this.userId = userId;
		console.log(this.userId);
	},
	methods: {
		// 微信号
		wechatChange(wcc) {
			this.wechat = wcc.detail.value;
		},

		// 修改微信号
		editWechatBtn(ew) {
			console.log(ew);
			let that = this;
			if (that.wechat) {
				uni.request({
					url: app.globalData.api + 'user/modify_info',
					method: 'post',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					data: {
						id: that.userId,
						key: that.editKey,
						val: that.wechat
					},
					success(wc) {
						console.log(wc);
						if (wc.data.status == 1) {
							uni.showLoading({
								title: '提交中...'
							});
							setTimeout(function() {
								uni.hideLoading();
								uni.reLaunch({
									url: '../member/member'
								});
							}, 300);
						} else {
							uni.showToast({
								title: wc.data.msg,
								icon: 'none'
							});
						}
					}
				});
			} else {
				uni.showToast({
					title: '请输入微信号',
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
