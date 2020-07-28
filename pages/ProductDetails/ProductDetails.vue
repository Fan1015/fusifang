<template>
	<view class="product-details">
		<view class="product-details-swiper">
			<!-- 轮播图 -->
			<swiper class="swiper-box" :circular="true" :indicator-dots="true" :autoplay="true" :interval="2000" :duration="500">
				<swiper-item v-for="(img, index) in goodsDetail.img_arr" :key="index">
					<view class="swiper-item"><image :src="img" mode="aspectFit"></image></view>
				</swiper-item>
			</swiper>
		</view>
		<!-- 轮播图 -->
		<!-- 价格 -->
		<view class="product-details-con">
			<view class="product-details-con-price" hover-class="none">
				¥{{ goodsDetail.price_yh }}
				<text>¥{{ goodsDetail.price }}</text>
			</view>
			<view class="product-details-con-line"></view>
			<view class="product-details-con-text" hover-class="none">
				<view>{{ goodsDetail.name }}</view>
				<view>{{ goodsDetail.intro }}</view>
			</view>
		</view>
		<!-- 价格 -->
		<!-- 导航详情 -->
		
		<view class="product-details-foot">
			<view class="product-details-foot-nav">
				<view class="product-details-foot-nav-left"  hover-class="none">
					图文详情
				</view>
				<!-- <view class="product-details-foot-nav-right"  hover-class="none" :class="navIndex==2 ? 'navclass' : ''" @click="showNav(2)">
					商品详情
				</view> -->
			</view>
			<!-- <view class="product-details-foot-intro" v-if="navIndex==1"  hover-class="none">
				<image src="../../static/productdetails2.png" mode=""></image>
				<image src="../../static/productdetails2.png" mode=""></image>
			</view> -->
			<!-- <view class="product-details-foot-intro" v-if="navIndex==2"  hover-class="none">
				<view class="product-details-foot-intro-parameters">
					商品参数
					<text>Product Descripton</text>
				</view>
				<m-table />
			</view> -->
			<view class="product-details-foot-intro" hover-class="none"><rich-text :nodes="goodsDetail.content"></rich-text></view>
		</view>
		
		<!-- 导航详情 -->
		<!-- 电话详情 -->
		<view class="product-details-tel-bg"></view>
		<view class="product-details-tel">
			<view class="product-details-tel-fix">
				<view class="product-details-tel-fix-con" @click="prodetakefu">
					<view class="product-details-tel-fix-con-img" hover-class="none"><image src="../../static/tel.jpg" mode=""></image></view>
					<view class="product-details-tel-fix-con-phone" hover-class="none">+{{ goodsDetail.phone }}</view>
				</view>
			</view>
		</view>
		<!-- 电话详情 -->
	</view>
</template>

<script>
const app = getApp();
export default {
	data() {
		return {
			navIndex: 1,
			intro1: '新贵轻奢8小时，60支锦眠贡缎四件套',
			intro2: '高密纯棉贡缎，入门奢享',
			goodsDetail: [] //商品详情
		};
	},
	onLoad(options) {
		let that = this;
		console.log(options);
		uni.request({
			url: app.globalData.api + 'Product/details',
			method: 'post',
			header: {
				'content-type': 'application/x-www-form-urlencoded'
			},
			data: {
				pro_id: options.proId
			},
			success(gdinfor) {
				console.log(gdinfor);
				that.goodsDetail = gdinfor.data.pro;
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
		//
		showNav(value) {
			this.navIndex = value;
		},
		// 电话
		prodetakefu() {
			uni.makePhoneCall({
				phoneNumber: '17737790922' //仅为示例
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.swiper-box {
	height: 620upx;
	background: rgba(255, 255, 255, 1);

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
	/deep/.uni-swiper-dots-horizontal {
		bottom: 40upx;
	}
}

.swiper-item {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100%;
	height: 620upx;
}

.swiper-item image {
	width: 100%;
	height: 100%;
}

.product-details-con {
	background-color: #fff;

	&-price {
		font-size: 48upx;
		color: rgba(255, 59, 48, 1);
		font-weight: 500;
		line-height: 50upx;
		padding: 19upx 0 19upx 32upx;

		> text {
			font-size: 22upx;
			font-weight: 400;
			color: rgba(179, 179, 179, 1);
			line-height: 30px;
			text-decoration: line-through;
			margin-left: 8upx;
		}
	}

	&-line {
		width: 686upx;
		height: 1upx;
		background: rgba(232, 232, 232, 1);
		margin: auto;
	}

	&-text {
		padding: 30upx 32upx 20upx;

		> view:first-child {
			font-size: 32upx;
			font-weight: 500;
			color: rgba(43, 48, 58, 1);
			line-height: 70upx;
		}

		> view:last-child {
			font-size: 28upx;
			font-weight: 400;
			color: rgba(96, 96, 96, 1);
			line-height: 40upx;
		}
	}
}

.product-details-foot {
	margin-top: 20upx;
	background-color: #fff;
	padding-top: 20upx;
	&-nav {
		width: 686upx;
		height: 70upx;
		border-radius: 36upx;
		border: 2upx solid rgba(51, 51, 51, 1);
		display: flex;
		overflow: hidden;
		margin: auto;
		&-left {
			width: 100%;
			text-align: center;
			font-size: 26upx;
			color: rgba(255, 255, 255, 1);
			line-height: 35px;
			letter-spacing: 1upx;
			background: rgba(55, 147, 223, 1);
		}
		&-right {
			width: 50%;
			text-align: center;
			font-size: 26upx;
			color: rgba(51, 51, 51, 1);
			line-height: 35px;
			letter-spacing: 1upx;
		}
		.navclass {
			color: rgba(255, 255, 255, 1);
			background: rgba(55, 147, 223, 1);
		}
	}

	&-intro {
		margin-top: 20upx;
		padding: 0 32upx;
		image {
			width: 100%;
		}

		&-parameters {
			font-size: 26upx;
			color: rgba(51, 51, 51, 1);
			line-height: 37upx;
			letter-spacing: 1upx;
			padding: 19upx 0 20upx 32upx;

			> text {
				font-size: 25upx;
				color: rgba(156, 156, 156, 1);
				line-height: 36upx;
				letter-spacing: 1upx;
				margin-left: 16upx;
			}
		}
	}
}
.product-details-tel-bg {
	height: 128upx;
}
.product-details-tel {
	position: fixed;
	bottom: 0;
	left: 0;
	width: 100%;
	padding: 20upx 0;
	background: rgba(255, 255, 255, 1);
	box-shadow: 0upx -1upx 0upx 0upx rgba(0, 0, 0, 0.5);
	.product-details-tel-fix {
		width: 686upx;
		height: 88upx;
		margin: auto;
		background: rgba(55, 147, 223, 1);
		border-radius: 10upx;
		color: rgba(255, 255, 255, 1);
		&-con {
			display: flex;
			justify-content: center;
			line-height: 105upx;
			image {
				width: 48upx;
				height: 48upx;
			}
			&-phone {
				line-height: 83upx;
				margin-left: 16upx;
			}
		}
	}
}
</style>
