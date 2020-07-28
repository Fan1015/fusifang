<template>
	<view class="mypromotioncode">
		<view class="mypromotioncode-bg">
			<image src="../../static/myPrCode1.png" mode=""></image>
			<view class="mypromotioncode-bg-con">
				<image src="../../static/myPrCode2.png" mode=""></image>
				<view class="mypromotioncode-bg-con-top" hover-class="none">
					<view class="mypromotioncode-bg-con-top-text">邀请码</view>
					<view class="mypromotioncode-bg-con-top-code">{{ codeValue.loginId }}</view>
					<view class="mypromotioncode-bg-con-top-copy" @click="copyText"><text>复制</text></view>
				</view>
				<view class="mypromotioncode-bg-con-foot" hover-class="none">
					<view class="mypromotioncode-bg-con-foot-img" @longpress="saveImg"><image :src="codeValue.rqcode" mode=""></image></view>
					<view class="mypromotioncode-bg-con-foot-preservation">长按保存二维码可分享</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
const app = getApp();
import '@/components/ican-H5Api/ican-H5Api.js';
export default {
	data() {
		return {
			codeValue: '', //邀请码
			userId: ''
		};
	},
	onLoad() {
		let that = this;
		let userId = uni.getStorageSync('userId');
		this.userId = userId;
		this.codeList()
	},
	// 下拉刷新
	onPullDownRefresh() {
		setTimeout(function() {
			uni.stopPullDownRefresh();
		}, 1000);
		this.codeList()
	},
	methods: {
		// 邀请码||二维码
		codeList(){
			let that = this;
			uni.request({
				url: app.globalData.api + 'user/user_info',
				method: 'post',
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				data: {
					id: that.userId
				},
				success(code) {
					console.log(code);
					that.codeValue = code.data.data;
				}
			});
		},
		// 复制
		copyText(res) {
			uni.setClipboardData({
				data: this.codeValue.loginId,
				success(res) {
					uni.showToast({
						title: '复制成功，立即分享',
						icon: 'none'
					});
				}
			});
		},
		// 长按保存二维码
		saveImg(long) {
			// console.log(long)
		}
	}
};
</script>

<style lang="scss" scoped>
page {
	// background:rgba(48,113,255,1);
}
/deep/element.style {
	background-position: 0% 0%;
	background-size: 100%;
	background-repeat: no-repeat;
	background-image: url(/static/myPrCode3.jpg);
}

.mypromotioncode {
	// position: relative;
	&-bg {
		width: 750upx;
		height: 1246upx;
		position: relative;

		image {
			width: 100%;
			height: 100%;
			position: absolute;
			top: 0;
			left: 0;
			z-index: 1;
		}

		&-con {
			width: 545upx;
			height: 640upx;
			position: absolute;
			bottom: 60upx;
			left: 102upx;
			z-index: 9;
			image {
				width: 100%;
				height: 100%;
			}
			&-top {
				position: absolute;
				bottom: 406upx;
				left: 80upx;
				text-align: center;
				z-index: 9;
				&-text {
					font-size: 28upx;
					font-weight: 400;
					color: rgba(144, 144, 144, 1);
					// line-height:40upx;
					letter-spacing: 1upx;
				}
				&-code {
					font-size: 66upx;
					font-weight: 600;
					color: rgba(57, 148, 223, 1);
					line-height: 92upx;
					letter-spacing: 4upx;
					margin: 10upx 0;
				}
				&-copy {
					width: 144upx;
					height: 53upx;
					background: linear-gradient(126deg, rgba(135, 192, 241, 1) 0%, rgba(55, 147, 223, 1) 100%);
					border-radius: 27upx;
					text-align: center;
					line-height: 45upx;
					margin: auto;
					> text {
						font-size: 26upx;
						font-weight: 400;
						color: rgba(255, 255, 255, 0.9);
						letter-spacing: 1upx;
					}
				}
			}
			&-foot {
				position: absolute;
				top: 45%;
				left: 150upx;
				z-index: 9;
				&-img {
					width: 240upx;
					height: 240upx;
					image {
						width: 100%;
						height: 100%;
					}
					/deep/uni-image {
						width: 240upx;
						height: 240upx;
					}
				}
				&-preservation {
					font-size: 24upx;
					font-weight: 400;
					color: rgba(187, 187, 187, 1);
					line-height: 33upx;
					letter-spacing: 1upx;
					margin-top: 40upx;
				}
			}
		}
	}
}
</style>
