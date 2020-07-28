<template>
	<view class="editPassword">
		<view class="editPassword-top">
			<view class="editPassword-top-h1">Hi，请输入转账信息</view>
			<view class="editPassword-top-h2">注意信息填写正确</view>
		</view>
		<view class="editPassword-bottom">
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">会员编号</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="text"
					@input="memberIdChange"
					value=""
					placeholder="请输入会员编号"
				/>
			</view>
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">转账金额</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="number"
					@input="transferAmountChange"
					value=""
					placeholder="请输入您的转账金额"
				/>
			</view>
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">二级密码</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="password"
					@input="zhuanzhangChange"
					value=""
					placeholder="请输入您的二级密码"
				/>
			</view>
		</view>
		<view class="editPassword-button"><button type="primary" @click="showText ? secondBtn() : transferBtn()">提交</button></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			memberIdNum: '', //会员编号
			transferAmount: '', //转账金额
			transferSecond: '', //二级密码
			userid: '',
			second: 30, //获取短信后倒计时
			showText: false //是否可点击
		};
	},
	onLoad(option) {
		console.log(option);
		console.log(this.second);
		this.editId = option.editId;
		let userId = uni.getStorageSync('userId');
		console.log(userId);
		this.userid = userId;
	},
	onShow() {
		
	},
	methods: {
		// 会员编号
		memberIdChange(newpass) {
			this.memberIdNum = newpass.detail.value;
		},

		// 转账金额
		transferAmountChange(confirm) {
			this.transferAmount = confirm.detail.value;
		},

		// 二级密码
		zhuanzhangChange(res) {
			this.transferSecond = res.detail.value;
		},

		// 提交
		transferBtn(newbtn) {
			let that = this;
			if (that.transferSecond && that.memberIdNum && that.transferAmount) {
				uni.request({
					url: app.globalData.api + 'user/userMoneyGive',
					method: 'post',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					data: {
						userId: that.userid,
						payPwd: that.transferSecond, // 二级密码
						loginId: that.memberIdNum, // 会员编号
						money: that.transferAmount // 转账金额
					},
					success(wmPrice) {
						console.log(wmPrice);
						if (that.transferSecond && wmPrice.data.data.status == 1) {
							uni.showLoading({
								title:'转账中...'
							})
							setTimeout(function(){
								uni.hideLoading()
								uni.switchTab({
									url: '../member/member'
								});
							},500)
							uni.showToast({
								title: wmPrice.data.data.msg,
								icon: 'none'
							});
							that.getCode();
						} else if (wmPrice.data.data.status == -1) {
							uni.showToast({
								title: '二级密码为空或' + wmPrice.data.data.msg,
								icon: 'none'
							});
						}
					}
				});
			}else{
				uni.showToast({
					title: '请完善信息后重试',
					icon: 'none'
				});
			}
		},
		// 倒计时提交
		secondBtn() {
			uni.showToast({
				title: '请30秒后重试！',
				icon: 'none'
			});
		},
		//倒计时30秒
		getCode: function() {
			let that = this;
			that.showText = false;
			var interval = setInterval(() => {
				that.showText = true;
				that.second--;
				if (that.second < 0) {
					that.second = 30;
					that.showText = false;
					clearInterval(interval);
				}
			}, 1000);
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
