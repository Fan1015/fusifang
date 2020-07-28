<template style="height: 100%;">
	<view style="overflow: hidden;position: relative;height: 100%;">
		<navigator url="../seach/seach" class="g_header">
			<view class="g_header_s"><image hover-class="none" style="height: 100%;width: 100%;" src="../../static/search icon.png" mode=""></image></view>
			<input class="g_h_input" hover-class="none" placeholder="请输入商品名称" type="text" />
		</navigator>
		<view class="g_main" hover-class="none">
			<view style="width:191upx ; display: fixed;">
				<scroll-view scroll-y="true">
					<view class="g_main_left">
						<text v-for="(n, index) in goodsListNavFirst" :key="index" :class="switchNum == index ? 'selected' : 'g_main_left_list'" @click="switch1(index, n.id)">
							{{ n.name }}
						</text>
					</view>
				</scroll-view>
			</view>

			<view class="g_main_right" style="" hover-class="none">
				<scroll-view scroll-y="true">
					<view class="g_main_right-c">
						<view class="g_main_right-c-t">为你推荐</view>
						<view class="g_main_right-c-b">
							<navigator class="g_main_right-c-b-l" v-for="(g, index) in goodsListNavSecond" :key="index" :url="'../goodsDetails/goodsDetails?id=' + g.id">
								<image :src="g.image" mode=""></image>
								<text class="ellipsis">{{ g.name }}</text>
							</navigator>
						</view>
					</view>
					<!-- <view class="g_main_right-c" v-if="switchNum == 1">234</view> -->
				</scroll-view>
			</view>
		</view>
	</view>
</template>

<script>
const app = getApp();
export default {
	components: {},
	data() {
		return {
			switchNum: 0,
			goodsListNavFirst: [], //商品一级分类
			page: 1, //分页
			goodsListNavSecond: [] //商品二级分类
		};
	},
	onLoad(option) {
		let that = this;
		console.log(option);
	},
	// 监听tab跳转
	onTabItemTap(tab) {
		let indexGoodsNum = uni.getStorageSync('goGoodsKey');
		let obj = {};
		obj.goodsId = this.goodsListNavFirst[0].id
		obj.res = 0
		uni.setStorageSync('goGoodsKey',obj)
		this.goodsListItem(indexGoodsNum.goodsId);
	},
	onShow() {
		let that = this;
		let indexGoodsNum = uni.getStorageSync('goGoodsKey');
		that.switchNum = indexGoodsNum.res;
		that.goodsListItem(indexGoodsNum.goodsId);
		// 商品一级分类
		uni.request({
			url: app.globalData.api + 'Category/getcat',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			data: {
				cat_id: ''
			},
			success(glnf) {
				console.log(glnf);
				that.goodsListNavFirst = glnf.data.catList;
			}
		});
	},
	// 下拉刷新
	onPullDownRefresh() {
		let indexGoodsNum = uni.getStorageSync('goGoodsKey');
		console.log(indexGoodsNum);
		this.switchNum = indexGoodsNum.res;
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		this.goodsListItem(indexGoodsNum.goodsId);
	},
	methods: {
		switch1(num, goodsId) {
			let indexGoodsNum = uni.getStorageSync('goGoodsKey');
			console.log(num, goodsId);
			let that = this;
			this.switchNum = num;
			that.goodsListItem(goodsId);
		},
		//商品二级导航
		goodsListItem: function(numId) {
			let that = this;
			console.log(numId);
			uni.request({
				url: app.globalData.api + 'Category/getcat',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					cat_id: numId
				},
				success(glpi) {
					console.log(glpi);
					that.goodsListNavSecond = glpi.data.catList;
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.g_header {
	width: 100%;
	display: flex;
	background-color: #fff;
	height: 126upx;
	align-items: center;
	position: fixed;
	padding: 0 27upx;
	z-index: 900;

	.g_header_s {
		position: absolute;
		width: 36upx;
		height: 36upx;
		left: 57upx;
		top: 40upx;
	}

	.g_h_input {
		// flex: 1;
		width: 80%;
		height: 80upx;
		background: rgba(247, 249, 248, 1);
		border-radius: 16px;
		padding-left: 97upx;
	}
}

.g_main {
	margin-top: 126upx;
	display: flex;

	// height: 1015upx;
	height: 100%;

	/deep/uni-scroll-view {
		height: 100%;
	}

	.g_main_left {
		/deep/.uni-scroll-view ::-webkit-scrollbar {
			/* 隐藏滚动条，但依旧具备可以滚动的功能 */
			display: none;
			width: 0;
			height: 0;
			color: transparent;
			background: transparent;
		}

		/deep/::-webkit-scrollbar {
			display: none;
			width: 0;
			height: 0;
			color: transparent;
			background: transparent;
		}

		overflow-y: hidden;
		overflow: auto;
		// position: fixed;
		position: absolute;
		overflow: scroll;
		// float: left;
		width: 191upx;
		// height: 1962upx;
		height: 90%;
		display: flex;
		flex-direction: column;
		background: #fff;
		padding-top: 77upx;
		align-items: center;

		.g_main_left_list {
			margin-bottom: 66upx;
			display: block;
			width: 147upx;
			height: 45upx;
			border-radius: 8upx;
			font-size: 26upx;
			font-family: PingFangSC-Regular, PingFang SC;
			font-weight: 400;
			color: rgba(94, 94, 94, 1);
			line-height: 37upx;
			text-align: center;
		}
	}
}

.g_main_right {
	margin-bottom: 106upx;

	/deep/.uni-scroll-view,
	/deep/.uni-scroll-view-content {
		// height: auto;
	}

	// flex: 1;
	padding: 20upx;

	/deep/uni-scroll-view {
		width: 509upx;
		background: #fff;
	}

	.g_main_right-c {
		.g_main_right-c-t {
			font-size: 26upx;
			font-family: PingFangSC-Semibold, PingFang SC;
			font-weight: 600;
			color: rgba(51, 51, 51, 1);
			line-height: 37upx;
			margin: 20upx;
		}

		.g_main_right-c-b {
			display: flex;
			flex-wrap: wrap;
			justify-content: flex-start;
			align-items: flex-start;
			.g_main_right-c-b-l {
				margin-bottom: 30upx;
				margin-right: 10upx;
				width: 30%;
				display: flex;
				flex-direction: column;
				align-items: center;
				font-size: 24upx;
				font-family: PingFangSC-Regular, PingFang SC;
				font-weight: 400;
				color: rgba(108, 105, 107, 1);
				text {
					width: 100%;
					text-align: center;
				}
				image {
					width: 150upx;
					height: 150upx;
				}
			}
		}
	}
}

.selected {
	// display: block;
	// width:147upx;
	// height:45upx;
	background: rgba(55, 147, 223, 1);
	// border-radius:8upx;
	line-height: 45upx;
	color: #fff;

	margin-bottom: 66upx;
	display: block;
	width: 147upx;
	height: 45upx;
	border-radius: 8upx;
	font-size: 26upx;
	font-family: PingFangSC-Regular, PingFang SC;
	font-weight: 400;
	// color: rgba(94, 94, 94, 1);
	// line-height: 37upx;
	text-align: center;
}

uni-page-body {
	height: 100%;
	overflow: auto;
}
</style>
