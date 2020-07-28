<template>
	<view class="index-content">
		<!-- 轮播图 -->
		<swiper class="swiper-box" :indicator-dots="true" :autoplay="false" :interval="3000" :duration="1000">
			<swiper-item v-for="(i, index) in indexImg" :key="index">
				<view class="swiper-item"><image :src="i.photo" mode=""></image></view>
			</swiper-item>
		</swiper>
		<!-- 轮播图 -->

		<!-- nav -->
		<view class="index-nav">
			<navigator url="../latestNews/latestNews" class="index-nav-item" hover-class="none">
				<image src="../../static/icon1.png" mode=""></image>
				<view class="index-nav-item-text">最新消息</view>
			</navigator>
			<navigator url="../aboutUs/aboutUs?title=平台介绍" class="index-nav-item" hover-class="none">
				<image src="../../static/icon2.png" mode=""></image>
				<view class="index-nav-item-text">平台介绍</view>
			</navigator>
			<navigator url="../aboutUs/aboutUs?title=关于我们" class="index-nav-item" hover-class="none">
				<image src="../../static/icon3.png" mode=""></image>
				<view class="index-nav-item-text">关于我们</view>
			</navigator>
			<view @click="kefu" class="index-nav-item" hover-class="none">
				<image src="../../static/icon4.png" mode=""></image>
				<view class="index-nav-item-text">联系我们</view>
			</view>
		</view>
		<!-- nav -->

		<!-- 列表第一块 -->
		<view class="index-block1 index-bolck plr24 mt24" v-if="indexGoods.length > 0">
			<view class="index-bolck-content">
				<view @click="goGoodsList1(0)" class="index-title arrow3">{{ blockanav }}</view>
			</view>
			<view class="index-block1-item plr24">
				<navigator :url="'../ProductDetails/ProductDetails?proId=' + blockaImgId" class="index-block1-item-left" hover-class="none">
					<image :src="blockaImg" mode=""></image>
					<!-- <text>{{indexGoods[0].photo_x}}</text> -->
				</navigator>
				<view class="index-block1-item-right" v-for="(blocka, index) in indexGoods" :key="index">
					<navigator :url="'../ProductDetails/ProductDetails?proId=' + blocka.id" hover-class="none" class="index-block1-item-right-link">
						<image :src="blocka.photo_x" mode=""></image>
						<view class="index-block1-item-right-link-text1 ellipsis">{{ blocka.name }}</view>
						<view class="index-block1-item-right-link-text2 ellipsis">{{ blocka.intro }}</view>
						<view class="index-block1-item-right-link-text3">
							¥{{ blocka.price_yh }}
							<text>¥{{ blocka.price }}</text>
						</view>
					</navigator>
				</view>
			</view>
		</view>
		<!-- 列表第一块 -->

		<!-- 列表第二块 -->
		<div class="index-block2 index-bolck plr24 mt24" v-if="indexGoodsb.length > 0">
			<view class="index-bolck-content">
				<view @click="goGoodsList1(1)" hover-class="none" class="index-title arrow3">{{ blockanbv }}</view>
			</view>
			<view class="index-block1-item plr24">
				<view class="index-block1-item-right" v-for="(blockb, index) in indexGoodsb" :key="index">
					<navigator :url="'../ProductDetails/ProductDetails?proId=' + blockb.id" hover-class="none" class="index-block1-item-right-link">
						<image :src="blockb.photo_x" mode=""></image>
						<view class="index-block1-item-right-link-text1 ellipsis">{{ blockb.name }}</view>
						<view class="index-block1-item-right-link-text2 ellipsis">{{ blockb.intro }}</view>
						<view class="index-block1-item-right-link-text3 index-block4-text">
							¥{{ blockb.price_yh }}
							<text v-if="isPriceShow">¥{{ blockb.price }}</text>
						</view>
					</navigator>
				</view>
			</view>
		</div>
		<!-- 列表第二块 -->

		<!-- 列表第三块 -->
		<view class="index-block3 index-bolck plr24 mt24" v-if="indexGoodsc.length > 0">
			<view class="index-bolck-content">
				<view @click="goGoodsList1(2)" hover-class="none" class="index-title arrow3">{{ blockancv }}</view>
			</view>
			<view class="index-block1-item index-block3-item plr24">
				<view class="index-block3-item-left">
					<navigator :url="'../ProductDetails/ProductDetails?proId=' + blockbImgId" class="index-block3-item-left-img" hover-class="none">
						<image :src="blockbImg" mode=""></image>
					</navigator>
					<view class="index-block1-item-right" v-for="(blockc, index) in indexGoodsc" :key="index">
						<navigator :url="'../ProductDetails/ProductDetails?proId=' + blockc.id" hover-class="none" class="index-block1-item-right-link">
							<image :src="blockc.photo_x" mode=""></image>
							<view class="index-block1-item-right-link-text1 ellipsis">{{ blockc.name }}</view>
							<view class="index-block1-item-right-link-text2 ellipsis">{{ blockc.intro }}</view>
							<view class="index-block1-item-right-link-text3">
								¥{{ blockc.price_yh }}
								<text>¥{{ blockc.price }}</text>
							</view>
						</navigator>
					</view>
				</view>
			</view>
		</view>
		<!-- 列表第三块 -->

		<!-- 列表第四快 -->
		<view class="index-block2 index-bolck plr24 mt24" v-if="indexGoodsd.length > 0">
			<view class="index-bolck-content">
				<view @click="goGoodsList1(3)" hover-class="none" class="index-title arrow3">{{ blockandv }}</view>
			</view>
			<view class="index-block1-item plr24">
				<view class="index-block1-item-right" v-for="(blockd, index) in indexGoodsd" :key="index">
					<navigator :url="'../ProductDetails/ProductDetails?proId=' + blockd.id" hover-class="none" class="index-block1-item-right-link">
						<image :src="blockd.photo_x" mode=""></image>
						<view class="index-block1-item-right-link-text3 index-block4-text">
							¥{{ blockd.price_yh }}
							<text>¥{{ blockd.price }}</text>
						</view>
					</navigator>
				</view>
			</view>
		</view>
		<!-- 列表第四快 -->
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			imgUrl: app.globalData.apiImg,
			isPriceShow: false,
			indexImg: [], //轮播图
			indexItem: [], //商品导航
			indexGoods: [], //第一块商品
			indexGoodsb: [], //第二块商品
			indexGoodsc: [], //第三块商品
			indexGoodsd: [], //第四块商品
			blockanav: '', //第一块导航
			blockanbv: '', //第一块导航
			blockancv: '', //第一块导航
			blockandv: '', //第一块导航
			blockaImg: '', //第一块第一张图片
			blockaImgId: '', //第一块第一张图片id
			blockbImg: '', //第三块第一张图
			blockbImgId: '' ,//第三块第一张图id
			kefuTel:''
		};
	},
	onLoad() {
		let that = this;
		this.goodsListFun();
		uni.request({
			url: app.globalData.api + 'Public/sysConfig',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			success(indextel) {
				console.log(indextel)
				that.kefuTel = indextel.data.data.phone
			}
		});
	},
	// 下拉刷新
	onPullDownRefresh() {
		console.log('refresh');
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		this.goodsListFun();
	},
	methods: {
		// 块1
		goGoodsList1(res){
			//res是当前传的第几块(0,1,2,3,4)
			console.log(this.indexItem)//导航数组
			let obj = {};
			obj.goodsId = this.indexItem[res].id
			obj.res = res
			uni.setStorageSync('goGoodsKey',obj)//保存点击当前更多的导航id和下标
			// console.log(2125,obj)
			uni.switchTab({
				url:'../goodsList/goodsList'
			})
		},
		
		// 联系客服
		kefu(res) {
			uni.makePhoneCall({
				phoneNumber: this.kefuTel //仅为示例
			});
		},

		// 商品接口
		goodsListFun() {
			let that = this;
			// 轮播图
			uni.request({
				url: app.globalData.api + 'Public/banner',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					position: 1
				},
				success(indexbanner) {
					that.indexImg = indexbanner.data.data;
				}
			});
			// 商品分类
			uni.request({
				url: app.globalData.api + 'Category/getcat',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				success(indexItemRes) {
					// console.log(indexItemRes);
					let iir = indexItemRes.data.catList.slice(0, 4);
					console.log(iir);
					// 每一块的导航title
					that.indexItem = iir;
					that.blockanav = iir[0].name;
					that.blockanbv = iir[1].name;
					that.blockancv = iir[2].name;
					that.blockandv = iir[3].name;
					// 第一块
					uni.request({
						url: app.globalData.api + 'Product/index',
						method: 'post',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						data: {
							cat_id: iir[0].id
						},
						success(indexgoodsRes) {
							console.log(888,indexgoodsRes)
							that.indexGoods = indexgoodsRes.data.pro.slice(1, 3);
							console.log(888,that.indexGoods,66666)
							that.blockaImg = indexgoodsRes.data.pro[0].photo_x;
							that.blockaImgId = indexgoodsRes.data.pro[0].id;
						}
					});
					// 第二块
					uni.request({
						url: app.globalData.api + 'Product/index',
						method: 'post',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						data: {
							cat_id: iir[1].id
						},
						success(indexgoodsResb) {
							console.log(indexgoodsResb);
							that.indexGoodsb = indexgoodsResb.data.pro.slice(0, 2);
						}
					});
					// 第三块
					uni.request({
						url: app.globalData.api + 'Product/index',
						method: 'post',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						data: {
							cat_id: iir[2].id
						},
						success(indexgoodsResb) {
							console.log(indexgoodsResb);
							that.indexGoodsc = indexgoodsResb.data.pro.slice(1, 5);
							that.blockbImg = indexgoodsResb.data.pro[0].photo_x;
							that.blockbImgId = indexgoodsResb.data.pro[0].id;
						}
					});
					// 第四块
					uni.request({
						url: app.globalData.api + 'Product/index',
						method: 'post',
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						data: {
							cat_id: iir[3].id
						},
						success(indexgoodsResb) {
							console.log(indexgoodsResb);
							that.indexGoodsd = indexgoodsResb.data.pro.slice(0, 2);
						}
					});
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.swiper-box {
	height: 280upx;
	background: rgba(255, 255, 255, 1);
	padding: 33upx 24upx;
	/deep/.uni-swiper-dot {
		width: 12upx;
		height: 12upx;
		border-radius: 100%;
		background: rgba(255, 255, 255, 0.4);
		transition: 1s;
	}
	/deep/.uni-swiper-dot-active {
		width: 48upx;
		height: 12upx;
		border-radius: 12upx;
		background: rgba(255, 255, 255, 1);
		transition: 1s;
	}
}
.swiper-item {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 702upx;
	height: 280upx;
	margin: auto;
	border-radius: 20upx;
	overflow: hidden;
}
.swiper-item image {
	width: 100%;
	height: 100%;
}

// 导航
.index-nav {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 20upx 63upx 32upx;
	background: rgba(255, 255, 255, 1);
	&-item {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		> image {
			width: 90upx;
			height: 90upx;
		}
		&-text {
			font-size: 24upx;
			color: rgba(51, 51, 51, 1);
			font-weight: 400;
			line-height: 33upx;
			margin-top: 16upx;
		}
	}
}
// 列表块
.index-title {
	font-size: 30upx;
	color: rgba(51, 51, 51, 1);
	font-weight: 500;
	line-height: 42upx;
}
.index-bolck {
	border-radius: 16upx 16upx 0 0;
}
.index-bolck:last-child {
	margin-bottom: 20upx;
}
.index-bolck-content {
	background: rgba(255, 255, 255, 1);
	border-radius: 16upx 16upx 0 0;
	padding: 24upx;
}
.index-block3-item {
	flex-direction: column;
	&-left {
		display: flex;
		justify-content: flex-start;
		align-items: flex-start;
		flex-wrap: wrap;
		.index-block1-item-right {
			margin-top: 24upx;
		}
		.index-block1-item-right:nth-child(2) {
			margin-top: 0;
		}
		&-img {
			width: 430upx;
			height: 299upx;
			margin-right: 16upx;
			> image {
				width: 100%;
				height: 100%;
				border-radius: 16px;
			}
		}
	}
}
.index-block3-right {
	margin-top: 24upx;
}
.index-block1-item {
	background: rgba(255, 255, 255, 1);
	display: flex;
	justify-content: flex-start;
	align-items: flex-start;
	padding-bottom: 24upx;
	border-radius: 0 0 16upx 16upx;
	&-left {
		width: 208upx;
		height: 299upx;
		margin-right: 16upx;
		> image {
			width: 100%;
			height: 100%;
			border-radius: 16upx;
		}
	}
	&-right {
		display: flex;
		justify-content: flex-start;
		align-items: flex-start;
		background: rgba(255, 255, 255, 1);
		&-link {
			width: 207upx;
			margin-right: 16upx;
			> image {
				width: 207upx;
				height: 160upx;
				border-radius: 16upx;
			}
			&-text1 {
				font-size: 30upx;
				color: rgba(51, 51, 51, 1);
				font-weight: 500;
				line-height: 42upx;
			}
			&-text2 {
				font-size: 26upx;
				color: rgba(153, 153, 153, 1);
				font-weight: 400;
				line-height: 37upx;
			}
			&-text3 {
				font-size: 36upx;
				color: rgba(255, 59, 48, 1);
				font-weight: 500;
				line-height: 50upx;
				> text {
					font-size: 22upx;
					color: rgba(179, 179, 179, 1);
					font-weight: 400;
					line-height: 30upx;
					text-decoration: line-through;
					margin-left: 8upx;
				}
			}
		}
		&-link:last-child {
			margin-right: 0;
		}
	}
}
.index-block4-text {
	text-align: center;
}
</style>
