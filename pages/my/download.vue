<template>
	<view class="download">
		<header-bar :title="i18n.header.header20"></header-bar>
		<view class="container">
			<view class="download-wrap">
				<view class="logo">
					<image src="../../static/images/icon32.png"></image>
					<text>{{env == 'ios'?i18n.my.lang58_2:i18n.my.lang58_1}}</text>
				</view>
				<view class="link">
					<text>{{env == 'ios'?address.ios_url:address.android_url}}</text>
				</view>
				<view class="download-qr" v-if="env">
					<view class="qrimg">
					    <tki-qrcode
					    ref="qrcode"
					    :cid="cid"
					    :val="env == 'ios'?address.ios_url:address.android_url"
					    :size="size"
					    :unit="unit"
					    :background="background"
					    :foreground="foreground"
					    :pdground="pdground"
					    :loadMake="loadMake"
					    :showLoading="showLoading"
					    :loadingText="loadingText"
					    />
					</view>
				</view>
				<view class="btn-wrap">
					<view class="btn-item gradient-blue" @tap="saveImage">{{i18n.index.lang12}}</view>
					<view class="btn-item gradient-violet" @tap="copyAddress">{{i18n.index.lang13}}</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue';
	export default {
		data() {
			return {
				address: {},
				env: 'android',
				cid: "1",
				size: 320,
				unit: 'upx',
				background: '#ffffff',
				foreground: '#000000',
				pdground: '#000000',
				showLoading: true,
				loadingText: '加载中...',
				loadMake: true,
			}
		},
		components:{
			HeaderBar
		},
		onLoad() {
			this.env = uni.getSystemInfoSync().platform
			this.getData()
		},
		methods: {
			getData(){
				this.uniRequest({
					url: 'download',
					method: 'GET'
				}).then(res => {
					this.address = res.result
					setTimeout(() => {
						this.$refs.qrcode._makeCode()
					}, 300)
				})
			},
			saveImage(){
				this.$refs.qrcode._saveCode()
			},
			copyAddress(){
				let { env, address } = this
				uni.setClipboardData({
				    data: env == 'ios'?address.ios_url:address.android_url,
				    success: function () {
				        uni.showToast({
							title: "复制成功"
						})
				    },
					fail: function() {
						uni.showToast({
							title: "保存失败，请稍后重试"
						});
					}
				});
			}
		},
		computed: {  
			i18n () {  
				return this.$t('index')  
			}  
		}
	}
</script>

<style lang="scss">
.download{
	.container{
		.download-wrap{
			background-color: $colorC;
			text-align: center;
			padding: 25rpx 30rpx 54rpx;
			box-shadow:0px 8rpx 8rpx 0px rgba(0,0,0,0.05);
			border-radius: 8rpx;
			.logo{
				display: flex;
				flex-direction: column;
				align-items: center;
				padding: 10rpx 0 32rpx;
				border-bottom: 1rpx solid #444444;
				image{
					width: 96rpx;
					height: 108rpx;
					margin-bottom: 20rpx;
				}
				text{
					font-size: $fontG;
					color: #04BED9;
				}
			}
			.link{
				font-size: $fontG;
				color: $colorA;
				padding-top: 34rpx;
				word-break: break-all;
			}
			.download-qr{
				width: 320rpx;
				height: 320rpx;
				padding: 20rpx;
				margin: 36rpx auto 0;
				background-color: $colorA;
				image{
					width: 320rpx;
					height: 320rpx;
				}
			}
			.btn-wrap{
				margin-top: 44rpx;
				display: flex;
				justify-content: space-between;
				.btn-item{
					width: 330rpx;
					height: 92rpx;
					line-height: 92rpx;
					border-radius: 8rpx;
					font-size: $fontH;
					color: $colorA;
					text-align: center;
					flex: 1;
					&:first-child{
						margin-right: 30rpx;
					}
				}
			}
		}
	}
}
</style>
