<template>
	<view class="recordManagement">
		<view class="recordManagement-top">
			<navigator :url="'../record/record?listId=11&recordInfor=serviceTotalMoney&moneyNum=' + reInfor.serviceTotalMoney + '&titleText=商业消费总金额'">
				<view class="recordManagement-top-h1">商业消费总金额（元）</view>
				<view class="recordManagement-top-price">{{ reInfor.serviceTotalMoney }}</view>
			</navigator>
			<view class="recordManagement-top-box">
				<navigator :url="'../record/record?listId=12&recordInfor=serviceMoney&moneyNum=' + reInfor.serviceMoney + '&titleText=商业消费金额'" class="recordManagement-top-box-item">
					<view class="recordManagement-top-box-item-h1">{{ reInfor.serviceMoney }}</view>
					<view class="recordManagement-top-box-item-h2">商业消费金额</view>
				</navigator>
				<navigator :url="'../record/record?listId=13&recordInfor=serviceEquity&moneyNum=' + reInfor.serviceEquity + '&titleText=商业股'" class="recordManagement-top-box-item">
					<view class="recordManagement-top-box-item-h1">{{ reInfor.serviceEquity }}</view>
					<view class="recordManagement-top-box-item-h2">商业股</view>
				</navigator>
				<navigator :url="'../record/record?listId=14&recordInfor=serviceExtraEquity&moneyNum=' + reInfor.serviceExtraEquity + '&titleText=剩余消费金额'" class="recordManagement-top-box-item">
					<view class="recordManagement-top-box-item-h1">{{ reInfor.serviceExtraEquity }}</view>
					<view class="recordManagement-top-box-item-h2">剩余消费金额</view>
				</navigator>
			</view>
		</view>
		<view class="recordManagement-bottom">
			<view class="recordManagement-bottom-item" v-for="(r, index) in rmInfor" :key="index">
				<view class="recordManagement-bottom-item-one">
					<view class="recordManagement-bottom-item-one-h1">会员编号</view>
					<view class="recordManagement-bottom-item-one-h2">{{ r.code }}</view>
				</view>
				<view class="recordManagement-bottom-item-one">
					<view class="recordManagement-bottom-item-one-h1">消费金额</view>
					<view class="recordManagement-bottom-item-one-h2">{{ r.money }}元</view>
				</view>
				<view class="recordManagement-bottom-item-one">
					<view class="recordManagement-bottom-item-one-h1">创建时间</view>
					<view class="recordManagement-bottom-item-one-h2">{{ r.createTime }}</view>
				</view>
			</view>
		</view>
		<!-- <view class="loading">{{ loadingText }}</view> -->
		<view class="recordManagement-buttonBg"></view>
		<view class="recordManagement-button"><button type="primary" hover-class="none" @click="accountBtn">立即录单</button></view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			userid: '', //用户ID
			reInfor: '', //录单金额信息
			rmInfor: '', //录单记录
			page: 1, //分页
			totalpage: 1 ,//总页数
			loadingText: '加载中...',
			timer:null
		};
	},
	onLoad(ontion) {
		let that = this;
		let userId = uni.getStorageSync('userId');
		console.log(userId);
		that.userid = userId;
		that.getMoreT();
	},
	onShow(ontion) {
		let that = this;
		uni.request({
			url: app.globalData.api + 'user/user_info',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			data: {
				id: that.userid
			},
			success(ui) {
				console.log(ui);
				that.reInfor = ui.data.data;
			}
		});
	},
	// 下拉刷新
	onPullDownRefresh() {
		console.log('refresh');
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		this.getMoreT();
	},
	// 上拉加载
	onReachBottom() {
		console.log(1)
		let that = this;
		if (that.timer != null) {
			clearTimeout(timer);
		}
		let timer = setTimeout(function() {
			that.getMore();
		}, 300);
	},
	methods: {
		// 立即充值
		accountBtn() {
			uni.navigateTo({
				url: '../recordManagementSecond/recordManagementSecond'
			});
		},
		getMore() {
			let that = this;
			console.log(that.page)
			uni.request({
				url: app.globalData.api + 'Record/index',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					userId: that.userid,
					p: that.page
				},
				success(rm) {
					console.log(rm);
					if (rm.data.data.root == '') {
						uni.hideNavigationBarLoading();
						uni.showToast({
							title:'已加载全部',
							icon:'none'
						})
						return false;
					}
					that.page ++ ;
					that.rmInfor = that.rmInfor.concat(rm.data.data.root);
					that.totalpage = rm.data.data.totalPage;
					uni.hideNavigationBarLoading();
				}
			});
		},
		getMoreT() {
			let that = this;
			console.log(that.page)
			uni.request({
				url: app.globalData.api + 'Record/index',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					userId: that.userid,
					p: 1
				},
				success(rm) {
					console.log(rm);
					that.page ++ ;
					that.rmInfor = rm.data.data.root;
					uni.hideNavigationBarLoading();
					uni.stopPullDownRefresh();
				}
			});
		},
		
	}
};
</script>

<style lang="scss" scoped>
.recordManagement {
	&-top {
		width: 100%;
		height: 343upx;
		background: rgba(55, 147, 223, 1);
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		position: relative;
		&-h1 {
			font-size: 26upx;
			color: rgba(255, 255, 255, 0.9);
			line-height: 37upx;
			font-weight: 400;
		}
		&-price {
			font-size: 66upx;
			line-height: 92upx;
			color: rgba(255, 255, 255, 0.9);
			font-weight: 600;
		}
		&-box {
			position: absolute;
			bottom: -100upx;
			width: 632upx;
			height: 102upx;
			padding: 48upx 27upx 46upx;
			background: rgba(255, 255, 255, 1);
			border-radius: 20upx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			&-item {
				display: flex;
				justify-content: center;
				align-items: center;
				flex-direction: column;
				&-h1 {
					font-size: 32upx;
					color: rgba(255, 0, 0, 1);
					line-height: 45upx;
					font-weight: 600;
				}
				&-h2 {
					font-size: 24upx;
					color: rgba(144, 144, 144, 1);
					line-height: 33upx;
					font-weight: 400;
					letter-spacing: 2upx;
					margin-top: 24upx;
				}
			}
		}
	}
	&-bottom {
		margin-top: 121upx;
		&-item:first-child {
			margin-top: 0;
		}
		&-item {
			background: rgba(255, 255, 255, 1);
			padding: 30px 32px;
			margin-top: 10upx;
			&-one:first-child {
				margin-top: 0;
			}
			&-one {
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-top: 20upx;
				&-h1 {
					font-size: 28upx;
					color: rgba(96, 96, 96, 1);
					font-weight: 400;
					line-height: 40upx;
				}
				&-h2 {
					font-size: 28upx;
					color: rgba(144, 144, 144, 1);
					font-weight: 400;
					line-height: 40upx;
				}
			}
		}
	}
	&-buttonBg {
		height: 138upx;
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
.loading {
	text-align: center;
	font-size: 32upx;
	color: rgba(128,128,128,1);
	font-weight: 400;
	line-height: 80px;
}
</style>
