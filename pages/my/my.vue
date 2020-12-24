<template>
	<view class="my">
		<header-bar :isBack="isBack" :isBg="isBg" :title="i18n.tabbar.my"></header-bar>
		<view class="content" :class="{'top': i18n.lang}">
			<view class="userinfo">
				<view class="head-image">
					<image src="../../static/images/icon32.png"></image>
				</view>
				<view class="head-right">
					<view class="phone">
						<text>{{userInfo.username}}</text>
					</view>
					<view class="name">
						<view class="txt">
							<text>{{userInfo.address}}</text>
						</view>
						<view class="btn" @tap="copyAddress">
							<text>{{i18n.my.lang59}}</text>
						</view>
					</view>
				</view>
			</view>
			<view class="operation-list-wrap">
				<view class="title blue">
					<text>{{i18n.my.lang44}}</text>
				</view>
				<view class="operation-box">
					<view class="operation-item" @tap="gotoPage(`modify?type=1&title=${i18n.my.lang72+i18n.header.header13}`)">
						<image class="pic1" src="../../static/images/icon33.png"></image>
						<text>{{i18n.header.header13}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage(`modify?type=2&title=${i18n.my.lang72+i18n.header.header14}`)">
						<image class="pic2" src="../../static/images/icon34.png"></image>
						<text>{{i18n.header.header14}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage(`modify?type=3&title=${i18n.my.lang72+i18n.header.header15}`)">
						<image class="pic3" src="../../static/images/icon35.png"></image>
						<text>{{i18n.header.header15}}</text>
					</view>
				</view>
			</view>
			<view class="operation-list-wrap">
				<view class="title purple">
					<text>{{i18n.my.lang45}}</text>
				</view>
				<view class="operation-box">
					<view class="operation-item" @tap="showLanguage">
						<image class="pic4" src="../../static/images/icon36.png"></image>
						<text>{{i18n.header.header16}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage('proposal')">
						<image class="pic5" src="../../static/images/icon37.png"></image>
						<text>{{i18n.header.header17}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage('newslist')">
						<image class="pic6" src="../../static/images/icon38.png"></image>
						<text>{{i18n.header.header18}}</text>
					</view>
				</view>
			</view>
			<view class="operation-list-wrap">
				<view class="title green">
					<text>{{i18n.my.lang46}}</text>
				</view>
				<view class="operation-box">
					<view class="operation-item" @tap="gotoPage('/pages/index/invitation')">
						<image class="pic7" src="../../static/images/icon39.png"></image>
						<text>{{i18n.header.header19}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage('download')">
						<image class="pic8" src="../../static/images/icon40.png"></image>
						<text>{{i18n.header.header20}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage('mywallet')">
						<image class="pic9" src="../../static/images/icon41.png"></image>
						<text>{{i18n.header.header21}}</text>
					</view>
				</view>
			</view>
			<view class="operation-list-wrap">
				<view class="title green">
					<text>{{i18n.my.lang47}}</text>
				</view>
				<view class="operation-box">
					<view class="operation-item" @tap="gotoPage('accounts')">
						<image class="pic10" src="../../static/images/icon42.png"></image>
						<text>{{i18n.header.header22}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage('team')">
						<image class="pic10" src="../../static/images/icon43.png"></image>
						<text>{{i18n.header.header23}}</text>
					</view>
					<view class="operation-item" @tap="gotoPage('authentication')">
						<image class="pic10" src="../../static/images/icon54.png"></image>
						<text>{{i18n.header.header27}}</text>
					</view>
				</view>
			</view>
			<view class="operation-btn-wrap">
				<view class="operation-btn gradient-blue" @tap="logOut">
					<text>{{i18n.my.lang48}}</text>
				</view>
				<view class="operation-btn gradient-green" @tap="logOut">
					<text>{{i18n.my.lang49}}</text>
				</view>
			</view>
		</view>
		<tabbar :isCurrent="5"></tabbar>
		<view class="mask" v-if="modalActive"></view>
		<view class="modal" :class="{'active': modalActive}">
			<view class="language-item" @tap="changeLanguage('zh_CN')">
				<text>简体中文</text>
			</view>
			<view class="language-item" @tap="changeLanguage('en_US')">
				<text>English</text>
			</view>
			<view class="language-cancel" @tap="cancelLanguage">
				<text>取消</text>
			</view>
		</view>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue'
	import Tabbar from '../../components/tabbar.vue'
	export default {
		data() {
			return {
				timer: '',
				isBack: false,    //头部返回按钮
				isBg: true,       //显示背景
				modalActive: false,    //modal弹出
				userInfo: '',
			}
		},
		components: {
			HeaderBar,
			Tabbar
		},
		onLoad() {
			let userInfo = uni.getStorageSync('userInfo')
			if(userInfo){
				this.userInfo = userInfo
			} else {
				uni.showToast({
					title: '请重新登录',
					icon: 'none',
					success: () => {
						setTimeout(() => {
							uni.navigateTo({
								url: '/pages/login/login'
							})
						}, 1000)
					}
				})
			}
		},
		methods: {
			gotoPage(url){
				uni.navigateTo({
				    url
				});
			},
			showLanguage(){
				this.modalActive = true
			},
			changeLanguage(language){
				const system_info = uni.getStorageSync('system_info')
				system_info.language = this._i18n.locale = language
				uni.setStorageSync('system_info',system_info)
				this.modalActive = false
			},
			moveHandle(){
				//禁止页面滚动
				if(this.modalActive){
					return
				}
			},
			cancelLanguage(){
				this.modalActive = false
			},
			logOut(){
				// 退出登录
				this.uniRequest({
					url: 'logOut',
					method: 'GET'
				}).then(res => {
					uni.reLaunch({
						url: '/pages/login/login'
					})
				})
			},
			copyAddress(){
				let { address } = this.userInfo
				uni.setClipboardData({
				    data: address,
				    success: function () {
				        uni.showToast({
							title: "复制成功",
						})
				    },
					fail: function() {
						uni.showToast({
							title: "保存失败，请稍后重试",
							icon: "none"
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
/* #ifdef H5 */
uni-page-body{
	padding-bottom: 100rpx;
}
/* #endif */
.my{
	min-height: 100%;
	.content {
		width: 100%;
		min-height: 100%;
		box-sizing: border-box;
		padding: 0 30rpx 30rpx;
		background: #292F42 url('../../static/images/bot.png') center top no-repeat;
		background-size: contain;
		&.top{
			.operation-list-wrap{
				height: auto;
				padding-bottom: 30rpx;
				.operation-box{
					align-items: flex-start;
				}
			}
		}
		.userinfo{
			display: flex;
			align-items: center;
			margin-bottom: 60rpx;
			.head-image{
				width: 90rpx;
				height: 102rpx;
				margin-right: 26rpx;
				image{
					width: 90rpx;
					height: 102rpx;
				}
			}
			.head-right{
				flex: 1;
				.phone{
					font-size: $fontH;
					color: $colorA;
					font-weight: 400;
				}
				.name{
					font-size: $fontI;
					color: $colorA;
					margin-top: 10rpx;
					display: flex;
					justify-content: space-between;
					align-items: center;
					.txt{
						overflow: hidden;
						height: 40rpx;
						line-height: 40rpx;
						word-break: break-all;
						padding-right: 20rpx;
					}
					.btn{
						font-size: $fontJ;
						color: $colorF;
						flex-shrink: 0;
					}
				}
			}
		}
		.operation-list-wrap{
			height: 208rpx;
			background-color: $colorC;
			border-radius: 8rpx;
			margin-bottom: 24rpx;
			.title{
				padding: 0 28rpx;
				height: 84rpx;
				line-height: 84rpx;
				font-size: $fontH;
				font-weight: 400;
				&.blue{
					color: #04BED9;
				}
				&.purple{
					color: #87BDFA;
				}
				&.green{
					color: #1EADAA;
				}
			}
			.operation-box{
				display: flex;
				justify-content: space-between;
				text-align: center;
				align-items: flex-start;
				.operation-item{
					flex: 1;
					text{
						display: block;
						font-size: $fontI;
						color: $colorA;
					}
					.pic1{
						width: 48rpx;
						height: 48rpx;
					}
					.pic2{
						width: 52rpx;
						height: 52rpx;
					}
					.pic3{
						width: 38rpx;
						height: 54rpx;
					}
					.pic4{
						width: 56rpx;
						height: 56rpx;
					}
					.pic5{
						width: 54rpx;
						height: 50rpx;
					}
					.pic6{
						width: 56rpx;
						height: 56rpx;
					}
					.pic7{
						width: 52rpx;
						height: 52rpx;
					}
					.pic8{
						width: 56rpx;
						height: 54rpx;
					}
					.pic9{
						width: 58rpx;
						height: 54rpx;
					}
					.pic10{
						width: 60rpx;
						height: 52rpx;
					}
				}
			}
		}
		.operation-btn-wrap{
			display: flex;
			justify-content: space-between;
			align-items: center;
			.operation-btn{
				height: 104rpx;
				line-height: 104rpx;
				border-radius: 8rpx;
				text-align: center;
				font-size: $fontG;
				color: $colorA;
				flex: 1;
				&:first-child{
					margin-right: 30rpx;
				}
			}
		}
	}
	.mask{
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0,0,0, .4);
		z-index: 998;
	}
	.modal{
		width: 100vw;
		padding-top: 46rpx;
		border-radius: 20rpx 20rpx 0rpx 0rpx;
		background-color: $colorC;
		position: fixed;
		bottom: 100rpx;
		z-index: 999;
		box-shadow: 0 0 20rpx #000000;
		transform: translateY(200%);
		transition: all 0.5s;
		&.active{
			display: block;
			transform: translateY(0);
			transition: all 0.5s;
		}
		.language-item{
			height: 114rpx;
			line-height: 114rpx;
			text-align: center;
			background-color: #292F42;
			font-size: $fontH;
			color: $colorA;
			margin-bottom: 20rpx;
			&:last-child{
				margin-bottom: 0;
			}
		}
		.language-cancel{
			height: 114rpx;
			line-height: 114rpx;
			text-align: center;
			background-color: #292F42;
			font-size: $fontH;
			color: $colorA;
			margin-top: 30rpx;
		}
	}
}
</style>
