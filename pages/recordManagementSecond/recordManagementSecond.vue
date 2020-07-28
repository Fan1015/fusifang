<template>
	<view class="editPassword">
		<view class="editPassword-top">
			<view class="editPassword-top-h1">Hi，请输入以下信息</view>
			<view class="editPassword-top-h2">注意信息填写</view>
		</view>
		<view class="editPassword-bottom">
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">会员编号</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="text"
					@input="memberNumber"
					value=""
					placeholder="请输入会员编号"
				/>
			</view>
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">消费金额</view>
				<input
					class="editPassword-bottom-item-input"
					placeholder-class="editPassword-bottom-item-place"
					type="text"
					@input="consumptionAmount"
					value=""
					placeholder="请输入您的消费金额"
				/>
			</view>
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-text">利润</view>
				<picker @change="calculationPickerChange" :value="index" :range="array" class="editPassword-bottom-item-input arrow4">
					<view class="uni-input" :class="[array[index] ? 'uni-input-active' : '']">{{ index != 0 ? consumptionamount / 100 * array[index]: '自动计算' }}</view>
				</picker>
			</view>
			<view class="editPassword-bottom-rate">{{array[index] == '自动计算' ? '请选择利润比例' : '*利润为消费金额的' + array[index] + '%'}}</view>
		</view>
		<view class="editPassword-button"><button type="primary" @click="showText ? showTextBtn() : newsInputBtn()">提交</button></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			membernumber: '', //会员编号
			consumptionamount: '', //消费金额
			array: [],
			index: 0,
			userId: '', //用户ID（会员Id）
			second: 30, //获取短信后倒计时
			showText: false,
		};
	},
	onLoad(option) {
		let that = this;
		let userId = uni.getStorageSync('userId');
		that.userId = userId;
		this.array.unshift('自动计算');
		uni.request({
			url: app.globalData.api + 'Public/sysConfig',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success(rms) {
				let rmsarr = rms.data.data.ludanbili.unshift('自动计算');
				that.array = rms.data.data.ludanbili
			}
		});
	},
	// 下拉刷新
	onPullDownRefresh() {
		console.log('refresh');
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
	},
	methods: {
		// 等30秒
		showTextBtn(){
			if(this.showText == true){
				uni.showToast({
					title:'请30秒后重试，还有' + this.second + '秒',
					icon:'none'
				})
			}
		},
		// 提交
		newsInputBtn() {
			let that = this;
			console.log(this.membernumber);
			console.log(this.consumptionamount);
			console.log(this.array[this.index]);
			
			if(that.membernumber){
				if(that.consumptionamount){
					if(this.array[this.index] != this.array[0]){
						uni.request({
							url: app.globalData.api + 'Record/submitOrder',
							method: 'post',
							header: {
								'content-type': 'application/x-www-form-urlencoded'
							},
							async:false,
							data:{
								userId:that.userId,
								code:that.membernumber,
								money:that.consumptionamount,
								scale:that.array[that.index]
							},
							success(newIn) {
								console.log(newIn)
								if(newIn.data.status == 1){
									uni.showLoading({
										title:"提交中..."
									})
									setTimeout(function(){
										uni.hideLoading()
										that.getCode()
										// uni.navigateBack({
										// 	delta:1
										// })
										that.membernumber = null
										that.consumptionamount = null
										that.index = 0
									},300)
								}else{
									uni.showToast({
										title:newIn.data.msg,
										icon:'none'
									})
								}
							}
						});
					}else{
						uni.showToast({
							title:'请选择利率',
							icon:'none'
						})
					}
				}else{
					uni.showToast({
						title:'请输入消费金额',
						icon:'none'
					})
				}
			}else{
				uni.showToast({
					title:'请输入会员编号',
					icon:'none'
				})
			}
			
		},
		//倒计时30秒
		getCode: function() {
			let that = this;
			that.showText = false;
			var interval = setInterval(() => {
				that.showText = true;
				that.second--;
				if (that.second < 0) {
					that.second == 0
					// setTimeout(() => {
					that.showText = false;
					clearInterval(interval);
					// }, 60000)
				}
			}, 1000);
		},
		calculationPickerChange(e) {
			console.log('picker发送选择改变，携带值为', e.target.value);
			this.index = e.target.value;
			console.log(this.index);
			// console.log(this.arry[this.index])
		},
		memberNumber(number) {
			this.membernumber = number.detail.value;
		},
		consumptionAmount(consumption) {
			this.consumptionamount = consumption.detail.value;
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
			color: rgba(55, 147, 223, 1);
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
			.uni-input {
				font-size: 34upx;
				color: rgba(187, 187, 187, 1);
				font-weight: 400;
				line-height: 48upx;
				letter-spacing: 2upx;
			}
			.uni-input-active {
				color: rgba(51, 51, 51, 1);
			}
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

		&-rate {
			font-size: 24upx;
			color: rgba(240, 84, 85, 1);
			line-height: 33upx;
			letter-spacing: 1upx;
			margin-top: 20upx;
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
