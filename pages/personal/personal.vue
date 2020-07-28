<template>
	<view class="personal">
		<view class="personal-top">
			<view class="personal-top-left">
				<view class="personal-top-left-h1">
					<text class="personal-top-left-h1-one" @click="copyText">{{ personalInfor.loginId }}</text>
					<text class="personal-top-left-h1-two" v-if="personalInfor.level == 1">服务站</text>
				</view>
				<view class="personal-top-left-copy">轻点复制</view>
				<view class="personal-top-left-h2">普通会员</view>
			</view>
			<view class="personal-top-right"><image :src="personalInfor.photo ? personalInfor.photo : '../../static/default.jpg'"></image></view>
		</view>
		<view class="personal-bottom">
			<view class="personal-bottom-item">
				<view class="personal-bottom-item-title">手机号</view>
				<view url="" hover-class="none" class="personal-bottom-item-number">{{ personalInfor.tel }}</view>
			</view>
			<view class="personal-bottom-item">
				<view class="personal-bottom-item-title">微信号</view>
				<navigator :url="'../editWechat/editWechat?key=' + 'wechat'" hover-class="none" class="personal-bottom-item-number arrow3">{{ personalInfor.wechat }}</navigator>
			</view>
			<view class="personal-bottom-item">
				<view class="personal-bottom-item-title">省份</view>
				<navigator :url="'../editAddress/editAddress?key=' + 'area'" hover-class="none" class="personal-bottom-item-number arrow3">{{ personalInfor.area }}</navigator>
			</view>
			<view class="personal-bottom-item">
				<view class="personal-bottom-item-title">登录密码</view>
				<navigator url="../editPassword/editPassword?editId=1" hover-class="none" class="personal-bottom-item-number arrow3">******</navigator>
			</view>
			<view class="personal-bottom-item">
				<view class="personal-bottom-item-title">二级密码</view>
				<navigator url="../editPassword/editPassword?editId=2" hover-class="none" class="personal-bottom-item-number arrow3">******</navigator>
			</view>
		</view>
	</view>
</template>

<script>
import pickerAddress from '@/components/wangding-pickerAddress/wangding-pickerAddress.vue';
import '@/components/ican-H5Api/ican-H5Api.js';
export default {
	data() {
		return {
			province: '请选择所在省份/城市/区', //省市区
			city: '', //市
			area: '', //区
			personalInfor: '' //个人信息
		};
	},

	onLoad(option) {
		this.personalInfor = JSON.parse(option.infor);
	},
	onShow(option) {
		let that = this;
		console.log(this.personalInfor);
	},
	// 下拉刷新
	onPullDownRefresh() {
		let that = this;
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		console.log(this.personalInfor)
		this.personalInfor
	},
	methods: {
		// 复制会员编号
		copyText(){
			uni.setClipboardData({
				data: this.personalInfor.loginId,
				success(res) {
					uni.showToast({
						title: '复制成功',
						icon: 'none'
					});
				}
			});
		}
	}
};
</script>

<style lang="scss" scoped>
.personal {
	&-top {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 64upx 32upx;
		background: rgba(255, 255, 255, 1);
		&-left {
			&-h1 {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				&-one {
					font-size: 48upx;
					font-weight: 600;
					color: rgba(51, 51, 51, 1);
					line-height: 67upx;
				}
				&-two {
					padding: 0upx 8upx;
					line-height: 36upx;
					margin-left: 20upx;
					font-size: 24upx;
					color: rgba(255, 255, 255, 1);
					background: rgba(55, 147, 223, 1);
					border-radius: 10upx;
				}
			}
			&-h2 {
				font-size: 30upx;
				font-weight: 400;
				color: rgba(55, 147, 223, 1);
				line-height: 42upx;
				margin-top: 23upx;
			}
			&-copy {
				font-size: 20upx;
				color: rgba(205, 205, 205, 1);
				font-weight: 400;
			}
		}
		&-right {
			width: 142upx;
			height: 142upx;
			image {
				width: 100%;
				height: 100%;
				border-radius: 100%;
			}
		}
	}
	&-bottom {
		padding: 30upx 38upx 30upx 32upx;
		background: rgba(255, 255, 255, 1);
		margin-top: 20upx;
		&-item {
			margin-bottom: 30upx;
			&-title {
				font-size: 28upx;
				color: rgba(110, 110, 110, 1);
				font-weight: 400;
				line-height: 40upx;
			}
			&-number {
				font-size: 34upx;
				color: rgba(64, 64, 64, 1);
				font-weight: 500;
				line-height: 48upx;
				margin-top: 25upx;
				padding-bottom: 30upx;
				border-bottom: 2upx solid rgba(229, 229, 229, 1);
			}
			&-pca {
				color: rgba(171, 168, 168, 1) !important;
			}
		}
	}
	&-button-bg {
		height: 117upx;
		width: 100%;
		background: rgba(255, 255, 255, 1);
	}
	&-button {
		position: fixed;
		bottom: 0upx;
		left: 0upx;
		width: 100%;
		background: rgba(255, 255, 255, 1);
		padding-bottom: 23upx;
		button {
			background: rgba(55, 147, 223, 1);
			width: 670upx;
			height: 94upx;
		}
	}
}
</style>
