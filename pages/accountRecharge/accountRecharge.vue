<template>
	<view class="accountRecharge">
		<view class="accountRecharge-top">
			<view class="accountRecharge-top-title">
				<text>请选择充值金额</text>
				<navigator url="../rechargeRecord/rechargeRecord?listId=10">查看明细</navigator>
			</view>
			<view class="accountRecharge-top-item">
				<view
					class="accountRecharge-top-item-text"
					v-for="(i, index) in accountPrice"
					:key="index"
					:class="[isSelect == i.pId ? 'accountRecharge-top-item-select' : '']"
					@click="topUpBtn(i)"
				>
					{{ i.priceNum }}元
				</view>
				<!-- <view class="accountRecharge-top-item-text" :class="[isSelect ? '' : '']">200元</view>
				<view class="accountRecharge-top-item-text" :class="[isSelect ? '' : '']">200元</view>
				<view class="accountRecharge-top-item-text" :class="[isSelect ? '' : '']">200元</view> -->
			</view>
		</view>
		<view class="accountRecharge-center">
			<text>请选择充值金额</text>
			<input type="number" :value="isSelectPrice" placeholder-class="accountRecharge-center-place" placeholder="输入其他充值金额,最低1元" @input="accountRechargeChange" />
		</view>
		<view class="accountRecharge-bottom">
			<image src="../../static/accountRecharge.png" mode=""></image>
			<text>微信支付</text>
		</view>
		<view class="accountRecharge-buttonBg"></view>
		<view class="accountRecharge-button"><button type="primary" hover-class="none" @click="accountBtn">立即充值</button></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			accountPrice: [
				{
					pId: 1,
					priceNum: 100
				},
				{
					pId: 2,
					priceNum: 300
				},
				{
					pId: 3,
					priceNum: 500
				},
				{
					pId: 4,
					priceNum: 800
				},
				{
					pId: 5,
					priceNum: 1000
				},
				{
					pId: 6,
					priceNum: 2000
				}
			], //充值金额选择
			defaultPrice: '',
			isSelect: 1, //充值选中状态
			isSelectPrice: 100, //充值金额
			isSelectId: 0, //选中的下标
			userid: ''
		};
	},
	onLoad() {
		let userId = uni.getStorageSync('userId');
		console.log(userId);
		this.userid = userId;
	},
	methods: {
		// 输入其他金额
		accountRechargeChange(ar) {
			this.isSelect = -1;
			this.isSelectPrice = ar.detail.value;
		},
		// 选择充值金额
		topUpBtn(res) {
			console.log(res);
			this.isSelectPrice = res.priceNum;
			this.isSelect = res.pId;
		},
		// 立即充值
		accountBtn(res) {
			let that = this;
			console.log(this.isSelectPrice);
			if (that.isSelectPrice) {
				uni.request({
					//充值
					url: app.globalData.api + 'user/recharge',
					method: 'post',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					data: {
						userId: that.userid,//当前用户id
						money: that.isSelectPrice//金额
					},
					success(req) {
						console.log(req);
						if (req.data.status == 1) {
							//调起微信支付
							uni.request({
								url: app.globalData.api + 'user/unifiedOrder',
								method: 'post',
								header: {
									'content-type': 'application/x-www-form-urlencoded'
								},
								data: {
									dataId: req.data.data,
									uid: that.userid
								},
								success(reqs) {
									console.log(reqs);
									console.log(JSON.parse(reqs.data.data));
									let wechatPayInfor = JSON.parse(reqs.data.data);
									WeixinJSBridge.invoke(
										'getBrandWCPayRequest',
										{
											appId: wechatPayInfor.appId, //公众号名称，由商户传入
											timeStamp: wechatPayInfor.timeStamp, //时间戳，自1970年以来的秒数
											nonceStr: wechatPayInfor.nonceStr, //随机串
											package: wechatPayInfor.package,
											signType: wechatPayInfor.signType, //微信签名方式：
											paySign: wechatPayInfor.paySign //微信签名
										},
										function(res) {
											console.log(res);
											if (res.err_msg == 'get_brand_wcpay_request:ok') {
												// 使用以上方式判断前端返回,微信团队郑重提示：
												//res.err_msg将在用户支付成功后返回ok，但并不保证它绝对可靠。
												uni.showToast({
													title: '支付成功',
													icon: 'none'
												});
												uni.navigateTo({
													url: '../myorder/myorder'
												});
											} else if (res.err_msg == 'get_brand_wcpay_request:cancel') {
												uni.showToast({
													title: '取消支付',
													icon: 'none'
												});
											}
										}
									);
								}
							});
						} else {
							uni.showToast({
								title: req.data.msg,
								icon: 'none'
							});
						}
					}
				});
			} else {
				uni.showToast({
					title: '请选择充值金额',
					icon: 'none'
				});
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.accountRecharge {
	&-top {
		padding: 30upx 32upx;
		background: rgba(255, 255, 255, 1);
		&-title {
			display: flex;
			justify-content: space-between;
			align-items: center;
			> text {
				font-size: 28upx;
				color: rgba(46, 50, 63, 1);
				font-weight: 400;
				line-height: 40upx;
			}
			> navigator {
				font-size: 28upx;
				color: rgba(128, 128, 128, 1);
				font-weight: 400;
				line-height: 40upx;
			}
		}
		&-item {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			flex-wrap: wrap;
			&-text {
				width: 214upx;
				background: rgba(229, 233, 240, 1);
				border-radius: 8upx;
				font-size: 32upx;
				color: rgba(96, 96, 96, 1);
				font-weight: 400;
				line-height: 81upx;
				text-align: center;
				margin-right: 20upx;
				margin-top: 20upx;
			}
			&-text:nth-child(3n) {
				margin-right: 0;
			}
			&-select {
				background: rgba(55, 147, 223, 1);
				color: rgba(255, 255, 255, 1);
			}
		}
	}
	&-center {
		padding: 0 32upx;
		margin: 40upx 0;
		display: flex;
		justify-content: space-between;
		align-items: center;
		> text {
			width: 196upx;
			font-size: 28upx;
			color: rgba(46, 50, 63, 1);
			font-weight: 400;
			line-height: 40upx;
		}
		> input {
			flex: 1;
			font-size: 28upx;
			color: rgba(46, 50, 63, 1);
			font-weight: 400;
			line-height: 40upx;
			text-align: right;
		}
		&-place {
			font-size: 28upx;
			color: rgba(128, 128, 128, 1);
			font-weight: 400;
			line-height: 40upx;
			text-align: right;
		}
	}
	&-bottom {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		padding: 32upx;
		background: rgba(255, 255, 255, 1);
		image {
			width: 48upx;
			height: 48upx;
			margin-right: 20upx;
		}
		text {
			font-size: 32upx;
			color: rgba(46, 50, 63, 1);
			font-weight: 400;
			line-height: 45upx;
		}
	}
	&-buttonBg {
		height: 128upx;
	}
	&-button {
		position: fixed;
		bottom: 0upx;
		left: 0upx;
		background: rgba(255, 255, 255, 1);
		padding: 17upx 40upx;
		box-shadow: 0px -1px 0px 0px rgba(0, 0, 0, 0.2);
		button {
			background: rgba(55, 147, 223, 1);
			width: 670upx;
			height: 94upx;
		}
	}
}
</style>
