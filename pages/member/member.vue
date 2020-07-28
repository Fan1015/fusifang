<template>
	<view class="member">
		<view class="member-box plr24">
			<view class="member-box-userinfor">
				<view class="member-box-userinfor-left">
					<view class="member-box-userinfor-left-img">
						<navigator :url="''" class="member-box-userinfor-left-img-txt" v-if="!userInfo.photo">绑定微信</navigator>
						<image :src="userInfo.photo" mode=""></image>
					</view>
					<view class="member-box-userinfor-left-username">
						<view class="member-box-userinfor-left-username-number">
							<text class="member-box-userinfor-left-username-number-one" @click="copyText()">{{ userInfo.loginId }}</text>
							<text class="member-box-userinfor-left-username-number-two" v-if="userInfo.level == 1">服务站</text>
						</view>
						<view class="member-box-userinfor-left-username-copy">轻点复制</view>
						<view class="member-box-userinfor-left-username-level" v-if="userId">普通会员</view>
					</view>
				</view>
				<navigator v-if="userId" class="member-box-userinfor-right" :url="'../personal/personal?infor=' + JSON.stringify(userInfo)">
					<image src="../../static/memberright.png" mode=""></image>
				</navigator>
			</view>
			<view class="member-box-datainfor">
				<navigator
					class="member-box-datainfor-item"
					:url="'../record/record?listId=1&recordInfor=userTotalMoney&moneyNum=' + userInfo.userTotalMoney + '&titleText=总消费金额'"
				>
					<view class="member-box-datainfor-item-number">{{ userInfo.userTotalMoney }}</view>
					<view class="member-box-datainfor-item-text">总消费金额</view>
				</navigator>
				<navigator class="member-box-datainfor-item" :url="'../record/record?listId=2&recordInfor=userMoney&moneyNum=' + userInfo.userMoney + '&titleText=消费金额'">
					<view class="member-box-datainfor-item-number">{{ userInfo.userMoney }}</view>
					<view class="member-box-datainfor-item-text">消费金额</view>
				</navigator>
				<navigator class="member-box-datainfor-item" :url="'../record/record?listId=3&recordInfor=userEquity&moneyNum=' + userInfo.userEquity + '&titleText=消费股'">
					<view class="member-box-datainfor-item-number">{{ userInfo.userEquity }}</view>
					<view class="member-box-datainfor-item-text">消费股</view>
				</navigator>
				<navigator
					class="member-box-datainfor-item"
					:url="'../record/record?listId=4&recordInfor=userExtraEquity&moneyNum=' + userInfo.userExtraEquity + '&titleText=剩余消费金额'"
				>
					<view class="member-box-datainfor-item-number">{{ userInfo.userExtraEquity }}</view>
					<view class="member-box-datainfor-item-text">剩余消费金额</view>
				</navigator>
			</view>
		</view>
		<!-- 模块一 -->
		<view class="member-box1">
			<navigator :url="'../myPurse/myPurse?infor=' + JSON.stringify(userInfo)" hover-class="none" class="member-box1-item arrow4">
				<view class="member-box1-item-img"><image src="../../static/membericon1.png" mode=""></image></view>
				<view class="member-box1-item-text">我的钱包</view>
				<view class="member-box1-item-text1">查看详情</view>
			</navigator>
			<navigator url="../transfer/transfer" hover-class="none" class="member-box1-item arrow4">
				<view class="member-box1-item-img"><image src="../../static/membericon4.png" mode=""></image></view>
				<view class="member-box1-item-text">转账</view>
			</navigator>
			<navigator url="../accountRecharge/accountRecharge" hover-class="none" class="member-box1-item arrow4">
				<view class="member-box1-item-img"><image src="../../static/membericon5.png" mode=""></image></view>
				<view class="member-box1-item-text">用户充值</view>
			</navigator>
		</view>
		<!-- 模块一 -->

		<!-- 模块二 -->
		<view class="member-box1">
			<navigator :url="'../myTeam/myTeam?level=' + userInfo.level" hover-class="none" class="member-box1-item arrow4">
				<view class="member-box1-item-img"><image src="../../static/membericon6.png" mode=""></image></view>
				<view class="member-box1-item-text">我的团队</view>
			</navigator>
			<navigator url="../myPromotionCode/myPromotionCode" hover-class="none" class="member-box1-item arrow4">
				<view class="member-box1-item-img"><image src="../../static/membericon7.png" mode=""></image></view>
				<view class="member-box1-item-text">我的推广码</view>
			</navigator>
			<navigator :url="'../recordManagement/recordManagement'" hover-class="none" class="member-box1-item arrow4" v-if="userInfo.level == 1">
				<view class="member-box1-item-img"><image src="../../static/membericon8.png" mode=""></image></view>
				<view class="member-box1-item-text">录单管理</view>
			</navigator>
		</view>
		<!-- 模块二 -->

		<!-- 模块三 -->
		<view class="member-box1">
			<navigator url="../aboutUs/aboutUs" hover-class="none" class="member-box1-item arrow4">
				<view class="member-box1-item-img"><image src="../../static/membericon10.png" mode=""></image></view>
				<view class="member-box1-item-text">关于我们</view>
			</navigator>
			<view @click="kefu" hover-class="none" class="member-box1-item arrow4">
				<view class="member-box1-item-img"><image src="../../static/membericon11.png" mode=""></image></view>
				<view class="member-box1-item-text">联系客服</view>
			</view>
		</view>
		<!-- 模块三 -->
		<!-- 退出登录 -->
		<view class="outUser" v-if="userId"><button type="primary" @click="outLogin">退出登录</button></view>
	</view>
</template>

<script>
const app = getApp();
import '@/components/ican-H5Api/ican-H5Api.js';
export default {
	data() {
		return {
			isPriceShow: false,
			userId: '', //用户ID（会员Id）
			userInfo: '', //用户信息
			kefuTel: ''
		};
	},
	onLoad() {
		let that = this;
		uni.request({
			url: app.globalData.api + 'Public/sysConfig',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success(indextel) {
				console.log(indextel);
				that.kefuTel = indextel.data.data.phone;
			}
		});
	},
	onShow() {
		let that = this;
		let userId = uni.getStorageSync('userId');
		// console.log(userId,2222223333)
		that.userId = userId;
		console.log(userId);
		if (!that.userId) {
			uni.navigateTo({
				url: '../login/login'
			});
		}
		uni.request({
			url: app.globalData.api + 'user/user_info',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			data: {
				id: userId
			},
			success(ui) {
				console.log(ui);
				that.userInfo = ui.data.data;
			}
		});
	},
	// 下拉刷新
	onPullDownRefresh() {
		let that = this;
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		//根据用户id去请求用户个人资料
		uni.request({
			url: app.globalData.api + 'user/user_info',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			data: {
				id: that.userId
			},
			success(ui) {
				console.log(ui);
				that.userInfo = ui.data.data;
			}
		});
	},
	methods: {
		// 联系客服
		kefu(res) {
			uni.makePhoneCall({
				phoneNumber: this.kefuTel //仅为示例
			});
		},

		// 复制会员编号
		copyText() {
			uni.setClipboardData({
				data: this.userInfo.loginId,
				success(res) {
					uni.showToast({
						title: '复制成功',
						icon: 'none'
					});
				}
			});
		},

		// 退出登录
		outLogin(ol) {
			let that = this;
			uni.showModal({
				title: '温馨提示',
				content: '您确定要退出登录吗？',
				success: res => {
					if (res.confirm) {
						uni.showLoading({
							title: ''
						});
						setTimeout(function() {
							uni.hideLoading();
							that.userId = '';
							uni.clearStorage();
							uni.showToast({
								title: '确定退出',
								icon: 'none'
							});
							uni.reLaunch({
								url: '../login/login'
							});
						}, 500);
					} else {
						uni.showToast({
							title: '取消退出',
							icon: 'none'
						});
					}
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.member-box {
	background: rgba(255, 255, 255, 1);
	&-userinfor {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 24upx 0;
		background: rgba(255, 255, 255, 1);
		border-bottom: 2upx solid rgba(238, 238, 238, 1);
		&-left {
			display: flex;
			justify-content: center;
			align-items: center;
			&-img {
				width: 120upx;
				height: 120upx;
				border-radius: 100%;
				margin-right: 24upx;
				image {
					width: 100%;
					height: 100%;
				}
				&-txt {
					font-size: 32upx;
					color: rgba(55, 147, 223, 1);
					width: 80upx;
					height: 80upx;
					padding: 20upx;
					font-weight: 400;
					background: rgba(55, 147, 223, 0.2);
					border-radius: 20upx;
					text-align: center;
					border: 2upx solid rgba(55, 147, 223, 1);
				}
			}
			&-username {
				&-number {
					display: flex;
					justify-content: flex-start;
					align-items: center;
					&-one {
						font-size: 36upx;
						color: rgba(51, 51, 51, 1);
						line-height: 50upx;
						font-weight: 400;
					}
					&-two {
						padding: 0upx 8upx;
						line-height: 36upx;
						margin-left: 20upx;
						font-size: 24upx;
						color: rgba(255, 255, 255, 1);
						background: rgba(55, 147, 223, 1);
						border-radius: 10upx;
					}
				}
				&-level {
					font-size: 30upx;
					color: rgba(55, 147, 223, 1);
					font-weight: 400;
					line-height: 42upx;
				}
				&-copy {
					font-size: 20upx;
					color: rgba(205, 205, 205, 1);
					font-weight: 400;
				}
			}
		}
		&-right {
			width: 32upx;
			height: 32upx;
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
	&-datainfor {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 24px 40upx;
		&-item {
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;
			&-number {
				font-size: 50upx;
				color: rgba(255, 0, 0, 1);
				line-height: 70upx;
				font-weight: 400;
			}
			&-text {
				font-size: 26upx;
				color: rgba(153, 153, 153, 1);
				font-weight: 400;
				line-height: 37upx;
			}
		}
	}
}
// 个人列表块
.member-box1:last-child {
	margin-bottom: 20upx;
}
.member-box1 {
	background: rgba(255, 255, 255, 1);
	border-top: 2upx solid rgba(238, 238, 238, 1);
	margin-top: 20upx;
	&-item {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		margin-left: 24upx;
		padding: 24upx 0;
		border-bottom: 2upx solid rgba(238, 238, 238, 1);
		position: relative;
		&-img {
			width: 56upx;
			height: 56upx;
			margin-right: 24upx;
			image {
				width: 100%;
				height: 100%;
			}
		}
		&-text {
			font-size: 34upx;
			color: rgba(51, 51, 51, 1);
			font-weight: 400;
			line-height: 48upx;
		}
		&-text1 {
			font-size: 30upx;
			color: rgba(153, 153, 153, 1);
			font-weight: 400;
			line-height: 42upx;
			position: absolute;
			top: 30upx;
			right: 74upx;
		}
	}
}
.outUser {
	margin: 20upx 0;
	button {
		width: 686upx;
		height: 88upx;
		background: #3793df;
	}
}
</style>
