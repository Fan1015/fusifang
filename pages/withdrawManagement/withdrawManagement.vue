<template>
	<view class="withdrawManagement">
		<view class="withdrawManagement-top">
			<image src="../../static/withdrawWechat.png" mode=""></image>
			<view class="withdrawManagement-top-text">提现到微信</view>
		</view>
		<view class="withdrawManagement-price">
			<view class="withdrawManagement-price-title">提现金额</view>
			<view class="withdrawManagement-price-num">
				<text>¥</text>
				<input type="number" value="" @input="withdrawPriceChange" />
			</view>
			<view class="withdrawManagement-price-withdrawable">可提现金额{{withdrawPriceX.whithdraw}}元</view>
		</view>
		<view class="withdrawManagement-bottom">
			*手续费{{withdrawPriceM.shouxu}}%（最低¥{{withdrawPriceM.lastMoney}}元可提现）
		</view>
		<view class="withdrawManagement-button">
			<button type="primary" hover-class="none" @click="withdrawBtn">确认提现</button>
		</view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			withdrawPrice: '' ,//提现金额
			withdrawPriceX:'',//剩余余额
			withdrawPriceM:'',
			userid:''
		};
	},
	onLoad(options) {
		let that = this
		let userId = uni.getStorageSync('userId');
		console.log(userId);
		this.userid = userId;
		this.withdrawPriceX = options
		uni.request({
			url: app.globalData.api + 'Public/sysConfig',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success(wm) {
				console.log(wm,12120);
				that.withdrawPriceM = wm.data.data
			}
		});
	},
	methods: {
		// 提现金额
		withdrawPriceChange(price) {
			this.withdrawPrice = price.detail.value;
		},
		// 确认提现
		withdrawBtn(res){
			let that = this;
			if(that.withdrawPrice){
				uni.request({
					url: app.globalData.api + 'user/userOnCash',
					method: 'post',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					data:{
						userId:that.userid,
						money:that.withdrawPrice
					},
					success(wmPrice) {
						console.log(wmPrice,202020);
						if(wmPrice.data.status == 1){
							uni.showLoading({
								title:'正在提现...'
							})
							setTimeout(function(){
								uni.hideLoading()
								uni.showToast({
									title:'提现成功',
									icon:'none'
								})
							},500)
							uni.navigateBack({
								delta:1
							})
						}else if(wmPrice.data.status == undefined){
							uni.showLoading({
								title:'提现失败，已转为线下提现!'
							})
							setTimeout(function(){
								uni.hideLoading()
								uni.navigateBack({
									delta:1
								})
							},500)
						}else{
							uni.showToast({
								title:wmPrice.data.msg,
								icon:'none'
							})
						}
						
					}
				});
			}else{
				uni.showToast({
					title:'请输入提现金额',
					icon:'none'
				})
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.withdrawManagement {
	&-top {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		padding: 29upx 32upx;
		background: rgba(255, 255, 255, 1);
		margin-top: 30upx;
		> image {
			width: 72upx;
			height: 72upx;
			margin-right: 27upx;
		}
		&-text {
			font-size: 30upx;
			font-weight: 400;
			color: rgba(51, 51, 51, 1);
			line-height: 42upx;
			letter-spacing: 2upx;
		}
	}
	&-price {
		padding: 57upx 32upx 20upx;
		background: rgba(255, 255, 255, 1);
		margin-top: 20upx;
		&-title {
			font-size: 30upx;
			color: rgba(51, 51, 51, 1);
			font-weight: 600;
			line-height: 42upx;
			letter-spacing: 2upx;
			margin-bottom: 66upx;
		}
		&-num {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			border-bottom: 2upx solid rgba(216, 216, 216, 1);
			padding-bottom: 25upx;
			text {
				font-size: 42upx;
				font-weight: 600;
				color: rgba(51, 51, 51, 1);
				line-height: 48upx;
				margin-right: 10upx;
			}
			input {
				width: 100%;
			}
		}
		&-withdrawable {
			font-size: 30upx;
			color: rgba(128, 128, 128, 1);
			font-weight: 400;
			line-height: 42upx;
			letter-spacing: 2upx;
			margin-top: 20upx;
		}
	}
	&-bottom{
		font-size: 24upx;
		color: rgba(240,84,85,1);
		font-weight: 400;
		line-height: 33upx;
		letter-spacing: 2upx;
		padding: 20upx 32upx 40upx;
	}
	&-button {
		width: 100%;
		padding-bottom: 23upx;
		button {
			background: rgba(55, 147, 223, 1);
			width: 670upx;
			height: 94upx;
		}
	}
}
</style>
