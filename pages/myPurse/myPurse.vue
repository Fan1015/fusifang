<template>
	<view class="myPurse">
		<view class="myPurse-top">
			<image src="../../static/myPurseimg.png" mode=""></image>
			<view class="myPurse-top-box">
				<view class="myPurse-top-box-title">当前余额</view>
				<view class="myPurse-top-box-price">
					{{ mtpurseInfor.money }}
					<text>元</text>
				</view>
				<view class="myPurse-top-box-bottom">
					<view class="myPurse-top-box-bottom-left">消费股：{{ mtpurseInfor.userEquity }}</view>
					<navigator :url="'../withdrawManagement/withdrawManagement?whithdraw=' + mtpurseInfor.money" class="myPurse-top-box-bottom-right">
						<image src="../../static/myPurseicon1.png" mode=""></image>
						<text>立即提现</text>
					</navigator>
				</view>
				<navigator :url="'../recordSecond/recordSecond?listId=7&recordInfor=totalMoney&moneyNum=' + mtpurseInfor.totalMoney + '&titleText=提现记录'" class="myPurse-top-box-withdrawals">
					提现记录
				</navigator>
			</view>
		</view>
		<view class="myPurse-bottom">
			<view class="myPurse-bottom-title">总金额（{{ mtpurseInfor.totalMoney }}元）</view>
			<view class="myPurse-bottom-nav">
				<text class="myPurse-bottom-nav-one">金额</text>
				<text class="myPurse-bottom-nav-two">来源与时间</text>
			</view>
			<view class="myPurse-bottom-content">
				<view class="myPurse-bottom-content-item" v-for="(rq, index) in purseIn" :key="index">
					<view class="myPurse-bottom-content-item-one" :class="[rq.dataType == 1 ? '' : 'myPurse-bottom-content-item-black']">
						<text class="myPurse-bottom-content-item-two-h2">{{ rq.dataType == 1 ? '+' + rq.money : '-' + rq.money }}</text>
					</view>
					<view class="myPurse-bottom-content-item-two">
						<text class="myPurse-bottom-content-item-two-h2">{{ rq.desc }}</text>
						<text class="myPurse-bottom-content-item-two-h2">{{ rq.createTime }}</text>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			mtpurseInfor: '',//当前用户信息
			purseIn: '',//明细
			userid: '',
			// 分页
			page: 1,
			totalPage: 1,
			timer: null
		};
	},
	onLoad(ontion) {
		let userId = uni.getStorageSync('userId');
		// console.log(userId);
		this.userid = userId;
	},
	onShow() {
		this.purseList()
	},
	// 下拉刷新
	onPullDownRefresh() {
		console.log('refresh');
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		this.purseList();
	},
	// 上拉加载
	onReachBottom() {
		let that = this;
		if (that.timer != null) {
			clearTimeout(timer);
		}
		let timer = setTimeout(function() {
			that.getMore();
		}, 300);
	},
	methods: {
		// 数据加载
		purseList(){
			let that = this;
			//余额明细
			uni.request({
				url: app.globalData.api + 'user/userMoneyDetailed',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					userId: this.userid,
					p: 1
				},
				success(req) {
					// console.log(req,2222);
					that.page++;
					that.purseIn = req.data.data.root;
					uni.hideNavigationBarLoading();
					uni.stopPullDownRefresh();
				}
			});
			
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
					console.log(ui,444);
					that.mtpurseInfor = ui.data.data;//用户信息
				}
			});
		},
		
		// 加载更多
		getMore(){
			let that = this;
			uni.request({
				url: app.globalData.api + 'user/userMoneyDetailed',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					userId: that.userid,
					p: that.page
				},
				success(req) {
					console.log(req);
					
					if (req.data.data.root == '') {
						uni.hideNavigationBarLoading();
						uni.showToast({
							title:'已加载全部',
							icon:'none'
						})
						return false;
					}
					that.page++;
					that.purseIn = that.purseIn.concat(req.data.data.root);
					that.totalpage = req.data.data.totalPage;
					uni.hideNavigationBarLoading();
				}
			});
			
		},
	}
};
</script>

<style lang="scss" scoped>
page {
	background: rgba(255, 255, 255, 1);
}
.myPurse {
	&-top {
		position: relative;
		height: 505upx;
		background: rgba(247, 247, 247, 1);
		> image {
			width: 100%;
			height: 304upx;
		}
		&-box {
			position: absolute;
			top: 150upx;
			left: 32upx;
			width: 646upx;
			padding: 60upx 20upx 20upx;
			margin: auto;
			background: rgba(255, 255, 255, 1);
			border-radius: 20upx;
			display: flex;
			justify-content: center;
			align-items: center;
			flex-direction: column;
			&-title {
				font-size: 32upx;
				color: rgba(96, 96, 96, 1);
				font-weight: 400;
				line-height: 45upx;
			}
			&-price {
				font-size: 46upx;
				color: rgba(46, 50, 63, 1);
				font-weight: 600;
				line-height: 78upx;
				margin-top: 19upx;
				text {
					font-size: 32upx;
					color: rgba(96, 96, 96, 1);
					font-weight: 400;
					line-height: 45upx;
				}
			}
			&-bottom {
				width: 100%;
				display: flex;
				justify-content: space-between;
				align-items: center;
				border-top: 2upx solid rgba(244, 247, 255, 1);
				margin-top: 65upx;
				padding-top: 20upx;
				&-left {
					font-size: 26upx;
					color: rgba(96, 96, 96, 1);
					font-weight: 400;
					line-height: 37upx;
				}
				&-right {
					display: flex;
					justify-content: flex-start;
					align-items: center;
					image {
						width: 32upx;
						height: 32upx;
					}
					text {
						font-size: 26upx;
						color: rgba(96, 96, 96, 1);
						font-weight: 400;
						line-height: 37upx;
					}
				}
			}
			&-withdrawals {
				width: 149upx;
				font-size: 26upx;
				color: rgba(255, 255, 255, 0.9);
				font-weight: 400;
				line-height: 49upx;
				text-align: center;
				border-radius: 25upx 0 0 25upx;
				background: rgba(129, 197, 254, 1);
				position: absolute;
				top: 144upx;
				right: 0;
			}
		}
	}
	&-bottom {
		background: rgba(255, 255, 255, 1);
		padding: 30upx 32upx;
		margin-top: 20upx;
		&-title:after {
			content: '';
			width: 83upx;
			height: 11upx;
			background: rgba(55, 147, 223, 1);
			position: absolute;
			bottom: 5upx;
			left: 0;
			z-index: -1;
		}
		&-title {
			font-size: 32upx;
			color: rgba(46, 50, 63, 1);
			font-weight: 600;
			position: relative;
			z-index: 9;
		}
		&-nav {
			display: flex;
			margin-top: 40upx;
			> text {
				font-size: 28upx;
				line-height: 33upx;
				font-weight: 400;
				color: rgba(144, 144, 144, 1);
			}
			&-one {
				width: 200upx;
			}
			&-two {
				width: 400upx;
			}
		}
		&-content {
			&-item {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				flex-wrap: wrap;
				border-bottom: 2upx solid rgba(232, 232, 232, 1);
				padding: 30upx 0;
				> view {
					font-size: 24upx;
					font-weight: 600;
					line-height: 40upx;
					display: flex;
					justify-content: flex-start;
					align-items: flex-start;
					flex-direction: column;
				}
				&-one {
					width: 200upx;
					color: rgba(247, 38, 22, 1);
				}
				&-black {
					color: rgba(51, 51, 51, 1);
				}
				&-two {
					width: 480upx;
					color: rgba(51, 51, 51, 1);
				}
			}
		}
	}
}
</style>
