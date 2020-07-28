<template>
	<view class="myteam">
		<view class="myteam-first" v-for="(items, index) in listDate" :key="index" v-if="listDate.length > 0">
			<view class="myteam-first-lf" hover-class="none">
				<view class="myteam-first-lf-img">
					<image :src="imgUrl + items.photo" mode="" v-if="items.photo && items.photo != null"></image>
					<image src="../../static/default.jpg" mode="" v-else></image>
				</view>
				<view class="myteam-first-lf-text">
					<view class="myteam-first-lf-text-h1">
						<text class="myteam-first-lf-text-h1-text" @click="copyText(index)">{{ items.loginId }}</text>
						<text class="member-box-userinfor-left-username-number-two" v-if="items.level == 1">服务站</text>
					</view>
					<view class="myteam-first-lf-text-copy">轻点复制</view>
					<view class="myteam-first-lf-text-h2">{{ items.tel }}</view>
				</view>
			</view>
			<view class="myteam-first-rg" hover-class="none">
				<text>{{ items.createTime }}</text>
			</view>
		</view>
		<view class="myPurse-bottom-default" v-if="listDate.length <= 0">暂无团队</view>
	</view>
</template>

<script>
const app = getApp();
import '@/components/ican-H5Api/ican-H5Api.js';
export default {
	data() {
		return {
			imgUrl: app.globalData.apiImg,
			listDate: [],
			userid: ''
		};
	},
	onLoad(option) {
		let that = this;
		let userId = uni.getStorageSync('userId');
		console.log(userId);
		that.userid = userId;
		this.temaList();
	},
	// 下拉刷新
	onPullDownRefresh() {
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		this.temaList();
	},
	methods: {
		// 复制会员编号
		copyText(index) {
			uni.setClipboardData({
				data: this.listDate[index].loginId,
				success(res) {
					uni.showToast({
						title: '复制成功',
						icon: 'none'
					});
				}
			});
		},
		// 团队接口
		temaList() {
			let that = this;
			uni.request({
				url: app.globalData.api + 'user/nextGroup',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					id: that.userid
					// level: option.level
				},
				success(team) {
					console.log(team);
					that.listDate = team.data.data;
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
page {
	background-color: #ffffff;
}

.myteam {
	&-first:first-child {
		margin-top: 0;
		padding-top: 29upx;
	}
	&-first {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-top: 59upx;

		&-lf {
			display: flex;
			align-items: center;
			margin-left: 31upx;

			&-img {
				width: 98upx;
				height: 98upx;
				margin-right: 19upx;

				image {
					width: 100%;
					height: 100%;
					border-radius: 100%;
				}
			}

			&-text {
				display: flex;
				flex-direction: column;
				&-copy {
					font-size: 20upx;
					color: rgba(205, 205, 205, 1);
					font-weight: 400;
				}
				&-h1 {
					&-text {
						font-size: 28upx;
						font-weight: 500;
						color: rgba(46, 50, 63, 1);
						line-height: 40upx;
					}
				}
				&-h2 {
					font-size: 24upx;
					font-weight: 400;
					color: rgba(134, 141, 145, 1);
					line-height: 33upx;
				}
			}
		}

		&-rg {
			margin-right: 32upx;

			> text {
				font-size: 24upx;
				font-weight: 400;
				color: rgba(144, 144, 144, 1);
				line-height: 33upx;
			}
		}
	}
}
.myPurse-bottom-default {
	font-weight: 400;
	font-size: 32upx;
	margin-top: 40upx;
	text-align: center;
	color: rgba(128, 128, 128, 1);
}
.member-box-userinfor-left-username-number-two {
	padding: 0upx 8upx;
	line-height: 36upx;
	margin-left: 20upx;
	font-size: 24upx;
	color: rgba(255, 255, 255, 1);
	background: rgba(55, 147, 223, 1);
	border-radius: 10upx;
}
</style>
