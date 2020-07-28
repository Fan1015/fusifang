<template>
	<view class="seach">
		<view class="g_header">
			<view class="g_header_s"><image hover-class="none" style="height: 100%;width: 100%;" src="../../static/search icon.png" mode=""></image></view>
			<input class="g_h_input" hover-class="none" placeholder="请输入商品名称" type="text" @input="searchBtn" />
		</view>

		<view class="d_main">
			<view class="d_main_l" v-if="searchGoods.length>0">
				<navigator :url="'../ProductDetails/ProductDetails?proId=' + s.id" v-for="(s,index) in searchGoods" :key="index">
					<view class="d_main_w_i"><image :src="s.photo_x" mode=""></image></view>
					<view class="d_main_w_m">
						<text>¥{{s.price_yh}}</text>
						<text>¥{{s.price}}</text>
					</view>
					<view class="s_t">{{s.name}}</view>
				</navigator>
			</view>
			<view class="seach-kong" v-else>还没有商品哦~</view>
		</view>
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			searchGoods:[],//搜索商品
		};
	},
	onLoad() {
		let that = this;
		uni.request({
			url: app.globalData.api + 'Product/index',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success(re) {
				console.log(re);
				that.searchGoods = re.data.pro
			}
		});
	},
	// 下拉刷新
	onPullDownRefresh() {
		setTimeout(function(){
			wx.stopPullDownRefresh();
		},1000)
	},
	methods: {
		// 搜索
		searchBtn(res) {
			let that = this;
			uni.request({
				url: app.globalData.api + 'Product/index',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					keyword: res.detail.value
				},
				success(re) {
					console.log(re);
					that.searchGoods = re.data.pro
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.g_header {
	width: calc(100% - 53upx);
	display: flex;
	background-color: #fff;
	height: 126upx;
	align-items: center;
	position: fixed;
	top: 0;
	padding: 0 27upx;
	z-index: 1000;

	.g_header_s {
		position: absolute;
		width: 36upx;
		height: 36upx;
		left: 57upx;
		top: 40upx;
	}

	.g_h_input {
		// flex: 1;
		width: 100%;
		height: 80upx;
		background: rgba(247, 249, 248, 1);
		border-radius: 16px;
		padding-left: 97upx;
	}
}
.d_main {
	margin-top: 40upx;
	padding: 24upx 20upx;
	display: flex;

	.d_main_l {
		justify-content: space-between;
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		// background-color: #fff;
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
.seach-kong {
	width: 100%;
	font-size: 30upx;
	text-align: center;
	color: rgba(128, 128, 128, 1);
	line-height: 60upx;
}
</style>
