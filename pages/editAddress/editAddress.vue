<template>
	<view class="editPassword">
		<view class="editPassword-top">
			<view class="editPassword-top-h1">Hi，请输入以下信息</view>
			<view class="editPassword-top-h2">注意信息填写正确</view>
		</view>
		<view class="editPassword-bottom">
			<view class="editPassword-bottom-item">
				<view class="editPassword-bottom-item-title">省份</view>
				<pickerAddress
					class="editPassword-bottom-item-number arrow3"
					:class="[province == '请选择所在省份/城市/区' ? 'editPassword-bottom-item-pca' : '']"
					@change="change"
				>
					{{ province }}{{ city }}{{ area }}
				</pickerAddress>
			</view>
		</view>
		<view class="editPassword-button"><button type="primary" @click="editWechatBtn">保存</button></view>
	</view>
</template>

<script>
const app = getApp();
import pickerAddress from '@/components/wangding-pickerAddress/wangding-pickerAddress.vue';
export default {
	data() {
		return {
			province: '请选择所在省份/城市/区', //省市区
			city: '', //市
			area: '', //区
			pca: '' //省市区集合
		};
	},
	components: { pickerAddress },
	onLoad(option) {
		console.log(option);
		this.editKey = option.key;
		let userId = uni.getStorageSync('userId');
		this.userId = userId;
		console.log(this.userId);
		console.log(this.editKey);
	},
	methods: {
		// 省市区
		change(data) {
			let that = this;
			console.log(data);
			that.province = data.data[0];
			that.city = data.data[1];
			that.area = data.data[2];
			that.pca = data.data[0] + data.data[1] + data.data[2];
		},
		// 保存
		editWechatBtn() {
			let that = this;
			if (that.pca) {
				uni.request({
					url: app.globalData.api + 'user/modify_info',
					method: 'post',
					header: {
						'content-type': 'application/x-www-form-urlencoded'
					},
					data: {
						id: that.userId,
						key: that.editKey,
						val: that.pca
					},
					success(wc) {
						console.log(wc);
						if (wc.data.status == 1) {
							uni.showLoading({
								title: '提交中...'
							});
							setTimeout(function() {
								uni.hideLoading();
								uni.reLaunch({
									url: '../member/member'
								});
							}, 300);
						} else {
							uni.showToast({
								title: wc.data.msg,
								icon: 'none'
							});
						}
					}
				});
			} else {
				uni.showToast({
					title: '请选择地址',
					icon: 'none'
				});
			}
		}
	}
};
</script>

<style lang="scss" scoped>
page {
	background: rgba(255, 255, 255, 1);
}
.editPassword {
	padding: 32upx;
	&-top {
		&-h1 {
			font-size: 46upx;
			color: rgba(0, 0, 0, 1);
			font-weight: 400;
			line-height: 65upx;
			letter-spacing: 3upx;
		}
		&-h2 {
			font-size: 24upx;
			color: rgba(102, 182, 112, 1);
			font-weight: 400;
			line-height: 33upx;
			letter-spacing: 2upx;
			margin-top: 20upx;
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
	&-button {
		margin-top: 100upx;
		button {
			background: rgba(55, 147, 223, 1);
			width: 670upx;
			height: 94upx;
		}
	}
}
</style>
