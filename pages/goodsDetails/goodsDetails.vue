<template>
	<view style="overflow: auto;">
		<!-- 导航 -->
		<scroll-view scroll-x="true" class="scroll-view-box">
			<view class="d_header" hover-class="none">
				<view :class="[switNum === index ? 'selected' : 'd_header_list']" @click="swit(index, gn.id)" v-for="(gn, index) in goodsDetailNav" :key="index">
					<view :class="[switNum === index ? 'select' : '']">{{ gn.name }}</view>
				</view>
			</view>
		</scroll-view>

		<!-- 商品列表 -->
		<view class="d_main" v-if="goodsDetailList.length > 0">
			<view class="d_main_l" hover-class="none">
				<navigator :url="'../ProductDetails/ProductDetails?proId=' + d.id" v-for="(d, index) in goodsDetailList" :key="index">
					<view class="d_main_w_i"><image :src="d.photo_x" mode=""></image></view>
					<view class="d_main_w_m">
						<text>¥{{ d.price_yh }}</text>
						<text>¥{{ d.price }}</text>
					</view>
					<view class="s_t ellipsis2">{{ d.name }}</view>
				</navigator>
			</view>
		</view>
		<view class="myPurse-bottom-default" v-else>暂无商品</view>
	</view>
</template>

<script>
const app = getApp();
export default {
	components: {},
	data() {
		return {
			switNum: 0,
			optionId: '', //商品id
			goodsDetailList: [], //商品列表
			goodsDetailNav: [] //商品导航
		};
	},
	onLoad(options) {
		let that = this;
		console.log(options);
		this.optionId = options.id;
		// 商品列表
		// this.goodsdetailNavfun(options.id);

		uni.request({
			url: app.globalData.api + 'Category/getcat',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			data: {
				cat_id: options.id
			},
			success(glnav) {
				console.log(glnav);
				that.goodsDetailNav = glnav.data.catList;
				uni.request({
					url: app.globalData.api + 'Product/index',
					method: 'post',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					data: {
						cat_id: glnav.data.catList[0].id
					},
					success(gd) {
						console.log(gd);
						that.goodsDetailList = gd.data.pro;
					}
				});
			}
		});
	},
	// 下拉刷新
	onPullDownRefresh() {
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
	},
	methods: {
		swit(num, gnId) {
			this.switNum = num;
			console.log(num, gnId);
			// 商品列表
			this.goodsdetailNavfun(gnId);
		},
		// 商品列表
		goodsdetailNavfun(goodsId) {
			console.log(goodsId);
			let that = this;
			uni.request({
				url: app.globalData.api + 'Product/index',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					cat_id: goodsId,
					keyword: '',
					page: 1
				},
				success(gd) {
					console.log(gd);
					that.goodsDetailList = gd.data.pro;
				}
			});
		},
		scrollX(res) {
			console.log(res);
		}
	}
};
</script>

<style lang="scss">
.scroll-view-box {
	white-space: nowrap;
	background: #fff;
}
.d_header {
	display: flex;
	align-items: center;
	padding: 0 32upx;
	height: 100upx;
	> view {
		display: inline-block;
		> text {
			box-sizing: border-box;
		}
	}
}

.d_main {
	padding: 24upx 20upx;
	display: flex;
	.d_main_l {
		justify-content: space-between;
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		navigator {
			width: 312upx;
			background-color: #fff;
			margin-bottom: 20upx;
			border-radius: 18upx;
			padding: 16upx;
			margin-right: 20upx;
		}
		navigator:nth-child(2n) {
			margin-right: 0upx;
		}
		.d_main_w_i {
			width: 100%;
			height: 300upx;
			margin-bottom: 16upx;
			image {
				width: 100%;
				height: 100%;
			}
		}
		.d_main_w_m {
			display: flex;
			align-items: center;
			text:nth-child(1) {
				font-size: 36upx;
				font-family: PingFangSC-Medium, PingFang SC;
				font-weight: 500;
				color: rgba(255, 59, 48, 1);
				line-height: 50upx;
				margin-right: 10upx;
			}
			text:nth-child(2) {
				text-decoration: line-through;
				font-size: 22upx;
				font-family: PingFangSC-Regular, PingFang SC;
				font-weight: 400;
				color: rgba(179, 179, 179, 1);
				line-height: 30upx;
			}
		}
		.s_t {
			width: 100%;
			height: 66upx;
			font-size: 24upx;
			font-family: PingFangSC-Regular, PingFang SC;
			font-weight: 400;
			color: rgba(98, 98, 98, 1);
			line-height: 30upx;
		}
	}
}
.d_header_list {
	height: 45upx;
	font-size: 32upx;
	font-family: PingFangSC-Regular, PingFang SC;
	font-weight: 400;
	color: rgba(209, 209, 214, 1);
	line-height: 45upx;
	margin-right: 66upx;
}
.selected {
	height: 45upx;
	font-size: 32upx;
	font-family: PingFangSC-Medium, PingFang SC;
	font-weight: 500;
	color: rgba(78, 76, 86, 1);
	line-height: 45upx;
	margin-right: 66upx;
	display: inline-block;
	border-bottom: 6upx solid rgba(51, 153, 255, 1);
}
</style>
