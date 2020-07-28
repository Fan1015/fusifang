<template>
	<view class="about-us">
		<view class="myPurse-top">
			<view class="myPurse-top-h1" @click="goPagePrve"></view>
			<view class="myPurse-top-h2">{{ titleTxt }}</view>
		</view>
		<view class="about-us-top">
			<image class="image1" src="../../static/aboutus1.png" mode=""></image>
			<view class="about-us-foot">
				<view class="about-us-foot-con" hover-class="none">
					<text class="about-us-foot-con-title">{{ title }}</text>
					<view class="about-us-foot-con-content" hover-class="none">
						<view class="about-us-foot-con-content-text">低属名地引入角满外光影角支比越只导干问候建型断即价两员矿以真军江人制确厂极位调况斗流别做养本三看还。</view>
						<view class="about-us-foot-con-content-text">程候积史才好经精看术改据那等人业置效书厂求下六效或新清因次低位或五都有动革层当人适位规府在求海上回织省族往率统往是军就八收二声东万青改运制放七美候者引即运许南门及动十金。。</view>
						<view class="about-us-foot-con-content-text">低属名地引入角满外光影角支比越只导干问候建型断即价两员矿以真军江人制确厂极位调况斗流别做养本三看还。</view>
						<view class="about-us-foot-con-content-text">程候积史才好经精看术改据那等人业置效书厂求下六效或新清因次低位或五都有动革层当人适位规府在求海上回织省族往率统往是军就八收二声东万青改运制放七美候者引即运许南门及动十金。</view>
					</view>
				</view>
			</view>
			<image class="image2" src="../../static/aboutus2.png" mode=""></image>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			title: '富四方商贸',
			titleTxt: ''
		};
	},
	onLoad(option) {
		console.log(option);
		this.titleTxt = option.title;
	},
	methods: {
		goPagePrve() {
			uni.navigateBack({
				delta: 1
			});
		}
	}
};
</script>

<style lang="scss" scoped>
page {
	background: #51a7ff;
}
.myPurse-top {
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
.about-us {
	&-top {
		.image1 {
			position: absolute;
			top: 88upx;
			left: 0;
			width: 100%;
			height: 1000upx;
		}
		.about-us-foot {
			width: 100%;
			position: relative;
			&-con {
				position: relative;
				top: 338upx;
				text-align: center;
				margin: 0 32upx;
				background: rgba(255, 255, 255, 1);
				border-radius: 20px;
				padding-top: 30upx;
				&-title {
					font-size: 32upx;
					color: rgba(0, 0, 0, 1);
					font-weight: 500;
					line-height: 45upx;
					letter-spacing: 7upx;
				}
				&-content {
					padding: 31upx 39upx;
					&-text {
						font-size: 24upx;
						color: rgba(96, 96, 96, 1);
						line-height: 40upx;
						text-indent: 40upx;
						text-align: left;
					}
				}
			}
		}
	}
}
.image2 {
	position: absolute;
	bottom: 0upx;
	width: 750upx;
	height: 200upx;
}
</style>
