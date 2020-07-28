<template>
	<view class="latestnews">
		<view class="latestnews-first" v-for="(items, index) in newsList" :key="index">
			<view class="latestnews-first-lf">
				<view class="latestnews-first-lf-img"><image src="../../static/news1.png" mode=""></image></view>
				<view class="latestnews-first-lf-text">
					<view class="latestnews-first-lf-text-text1">{{ items.articleTitle }}</view>
					<view class="latestnews-first-lf-text-text2">{{ items.createTime }}</view>
					<view class="latestnews-first-lf-text-text3">{{ items.articleContent }}</view>
					<!-- <rich-text :nodes="items.articleContent"></rich-text> -->
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
			newsList: []//最新消息
		};
	},
	onLoad() {
		let that = this;
		this.latestNewsList()
	},
	// 下拉刷新
	onPullDownRefresh() {
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		this.latestNewsList()
	},
	methods: {
		// 最新新闻接口
		latestNewsList(){
			let that = this;
			uni.request({
				url: app.globalData.api + 'public/articles',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					catId: 2
				},
				success(news) {
					console.log(news)
					that.newsList = news.data.data
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
page {
	background: #ffffff;
}
.latestnews {
	&-first:first-child {
		margin-top: 60upx;
	}
	&-first:last-child {
		border-bottom: none;
	}
	&-first {
		margin: 30upx 32upx 0 32upx;
		border-bottom: 1px solid rgba(247, 247, 247, 1);
		&-lf {
			display: flex;

			&-img {
				width: 90upx;
				height: 90upx;
				margin-right: 20upx;
				image {
					width: 100%;
					height: 100%;
				}
			}
			&-text {
				display: flex;
				flex-direction: column;
				width: 576upx;

				&-text1 {
					font-size: 32upx;
					font-weight: 400;
					color: rgba(51, 51, 51, 1);
					line-height: 45upx;
				}

				&-text2 {
					color: rgba(96, 96, 96, 1);
					line-height: 33upx;
					font-size: 24upx;
					font-weight: 400;
					margin: 8upx 0 16upx;
				}
				&-text3 {
					font-size: 24upx;
					font-weight: 400;
					color: rgba(128, 128, 128, 1);
					line-height: 33upx;
					margin-bottom: 30upx;
				}
			}
		}
	}
}
</style>
