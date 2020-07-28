<template>
	<view class="myPurse">
		<view class="myPurse-top">
			<view class="myPurse-top-h1" @click="goPagePrve"></view>
			<view class="myPurse-top-h2">{{ recordTitle }}</view>
		</view>
		<view class="myPurse-back"></view>
		<view class="myPurse-bottom" v-if="recordI.length > 0">
			<view class="myPurse-bottom-title">{{ recordText }}（{{ money.moneyNum }}）</view>
			<view class="myPurse-bottom-nav">
				<view class="myPurse-bottom-nav-one">金额</view>
				<view class="myPurse-bottom-nav-two">来源与时间</view>
			</view>
			<view class="myPurse-bottom-content">
				<view class="myPurse-bottom-content-item" v-for="(rs, index) in recordI" :key="index">
					<view class="myPurse-bottom-content-item-one" :class="[rs.dataType == 1 ? '' : 'myPurse-bottom-content-item-black']">
						<text class="myPurse-bottom-content-item-one-h2">{{ rs.dataType == 1 ? '+' + rs.money : '-' + rs.money }}</text>
					</view>
					<view class="myPurse-bottom-content-item-two">
						<text class="myPurse-bottom-content-item-two-h2">{{ rs.desc }}</text>
						<text class="myPurse-bottom-content-item-two-h2">{{ rs.createTime }}</text>
					</view>
				</view>
			</view>
		</view>
		<view class="myPurse-bottom-default" v-else>暂无记录</view>
	</view>
</template>

<script>
const app = getApp();
var timer = null;
export default {
	data() {
		return {
			recordTitle: '',
			recordText: '',
			recordInformation: '',
			userId: '',
			recordI: [],
			money: '',
			// 分页
			totalpage: 1,
			page: 1
			// loadingText: '加载中...'
		};
	},
	onLoad(options) {
		let that = this;
		let userId = uni.getStorageSync('userId');
		// console.log(userId);
		this.userid = userId;
		this.money = options;
		let recordIn = options.recordInfor;
		// console.log(recordIn);
		this.recordInformation = recordIn;
		this.recordTitle = options.titleText + '记录';
		this.recordText = options.titleText;
		this.getnewsList();
		// uni.request({
		// 	url: app.globalData.api + 'Record/logMoney',
		// 	method: 'post',
		// 	header: {
		// 		'content-type': 'application/x-www-form-urlencoded'
		// 	},
		// 	data: {
		// 		userId: userId,
		// 		mold: that.recordInformation,
		// 		p: 1
		// 	},
		// 	success(re) {
		// 		console.log(re);
		// 		uni.showLoading({
		// 			title: '加载中...'
		// 		});
		// 		setTimeout(function() {
		// 			uni.hideLoading();
		// 			that.recordI = re.data.data;
		// 		}, 300);
		// 	}
		// });
	},
	// 下拉刷新
	onPullDownRefresh() {
		setTimeout(function() {
			wx.stopPullDownRefresh();
		}, 1000);
		this.getnewsList();
	},
	// 上拉加载
	onReachBottom() {
		console.log(1);
		let that = this;
		// if (this.page * 1 < this.totalpage) {
		// 	console.log(2);
		// 	page++;
		// 	this.getMore();
		// } else {
		// 	console.log(3);
		// }
		if (that.timer != null) {
			clearTimeout(timer);
		}
		let timer = setTimeout(function() {
			that.getMore();
		}, 300);
	},
	methods: {
		// 返回上一层
		goPagePrve() {
			uni.navigateBack({
				delta: 1
			});
		},

		// 加载更多
		getMore() {
			let that = this;
			// if (that.loadingText != '' && that.loadingText != '加载更多') {
			// 	return false;
			// }
			// that.loadingText = '加载中...';
			uni.request({
				url: app.globalData.api + 'Record/logMoney',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					userId: that.userid,
					mold: that.recordInformation,
					p: that.page
				},
				success(re) {
					if (re.data.data.root == '') {
						uni.hideNavigationBarLoading();
						uni.showToast({
							title:'已加载全部',
							icon:'none'
						})
						return false;
					}
					that.page++;
					that.recordI = that.recordI.concat(re.data.data.root);
					that.totalpage = re.data.data.totalPage;
					uni.hideNavigationBarLoading();
				}
			});
		},

		getnewsList: function() {
			// let page = this.page;
			let that = this;
			uni.showNavigationBarLoading();
			uni.request({
				url: app.globalData.api + 'Record/logMoney',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					userId: that.userid,
					mold: that.recordInformation,
					p: 1
				},
				success: function(res) {
					that.page++;
					console.log(res);
					that.recordI = res.data.data.root;
					uni.hideNavigationBarLoading();
					uni.stopPullDownRefresh();
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
page {
	background: rgba(255, 255, 255, 1);
}
.myPurse {
	&-top {
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		height: 44px;
		height: calc(44px + constant(safe-area-inset-top));
		height: calc(44px + env(safe-area-inset-top));
		padding: 7px 3px;
		padding-top: calc(7px + constant(safe-area-inset-top));
		padding-top: calc(7px + env(safe-area-inset-top));
		display: -webkit-box;
		display: -webkit-flex;
		display: flex;
		align-items: center;
		justify-content: center;
		overflow: hidden;
		box-sizing: border-box;
		z-index: 998;
		-webkit-transition-property: all;
		transition-property: all;
		background: rgba(255, 255, 255, 1);
		&-h1 {
			position: relative;
			height: calc(44px + constant(safe-area-inset-top));
			height: calc(44px + env(safe-area-inset-top));
		}
		&-h1::before {
			content: '';
			width: 20upx;
			height: 20upx;
			border-top: 2upx solid rgba(0, 0, 0, 1);
			border-right: 2upx solid rgba(0, 0, 0, 1);
			transform: rotate(225deg);
			display: block;
			position: absolute;
			left: 32upx;
			top: 32upx;
		}
		&-h2 {
			width: 100%;
			font-size: 32upx;
			color: rgba(0, 0, 0, 1);
			line-height: 60upx;
			text-align: center;
			background-color: rgba(255, 255, 255, 1);
			font-weight: 700;
			overflow: hidden;
			white-space: nowrap;
			text-overflow: ellipsis;
		}
	}
	&-back {
		height: 88upx;
	}
	&-bottom {
		background: rgba(255, 255, 255, 1);
		padding: 30upx 32upx;
		&-title {
			font-size: 32upx;
			color: rgba(46, 50, 63, 1);
			font-weight: 600;
			position: relative;
			z-index: 9;
		}
		&-nav {
			display: flex;
			justify-content: flex-start;
			align-items: center;
			flex-wrap: wrap;
			margin-top: 40upx;
			> view {
				font-size: 28upx;
				line-height: 33upx;
				font-weight: 400;
				color: rgba(144, 144, 144, 1);
			}
			&-one {
				width: 200upx;
			}
			&-two {
				width: 480upx;
			}
		}
		&-content {
			&-item {
				display: flex;
				justify-content: flex-start;
				align-items: flex-start;
				flex-wrap: wrap;
				border-bottom: 2upx solid rgba(232, 232, 232, 1);
				padding: 30upx 0;
				> view {
					font-size: 24upx;
					font-weight: 400;
					line-height: 40upx;
					display: flex;
					justify-content: flex-start;
					align-items: flex-start;
					flex-direction: column;
				}
				&-one {
					width: 200upx;
					color: rgba(247, 38, 22, 1);
					&-h2 {
						margin-top: 20upx;
					}
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
.loading {
	text-align: center;
	line-height: 80px;
}
</style>
