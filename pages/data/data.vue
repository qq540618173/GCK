<template>
	<view class="data">
		<header-bar :isBack="isBack" :isBg="isBg" :title="i18n.tabbar.data"></header-bar>
		<view class="content">
			<view class="data-wrap">
				<view class="data-wrap-item">
					<image src="../../static/images/icon20.png"></image>
					<text class="label">{{i18n.data.lang32}}:</text>
					<text class="count">{{dataList.pool?dataList.pool.total:0.00}}</text>
				</view>
				<view class="data-wrap-item">
					<image src="../../static/images/icon21.png"></image>
					<text class="label">{{i18n.data.lang33}}:</text>
					<text class="count">{{dataList.pool?dataList.pool.market:0.00}}</text>
				</view>
				<view class="data-wrap-item">
					<image src="../../static/images/icon22.png"></image>
					<text class="label">{{i18n.data.lang34}}:</text>
					<text class="count">{{dataList.pool?dataList.pool.released:0.00}}</text>
				</view>
			</view>
			<view class="count-wrap">
				<view :class="i18n.data.lang35"></view>
				<view class="count-wrap-list">
					<view class="count-wrap-item">
						<view class="top">
							<text>{{i18n.data.lang36_1}}</text>
						</view>
						<view class="bot">
							<text>{{dataList.community?dataList.community.week:0.00}}</text>
						</view>
						<view class="tips">
							<text>{{i18n.data.data1}}{{dataList.day}}{{i18n.data.data2}}</text>
						</view>
					</view>
					<view class="count-wrap-item">
						<view class="top">
							<text>{{i18n.data.lang36}}</text>
						</view>
						<view class="bot">
							<text>{{dataList.community?dataList.community.month:0.00}}</text>
						</view>
					</view>
					<view class="count-wrap-item">
						<view class="top">
							<text>{{i18n.data.lang37}}</text>
						</view>
						<view class="bot">
							<text>{{dataList.community?dataList.community.quarter:0.00}}</text>
						</view>
					</view>
				</view>
				<view class="level">
					<view class="level-item">
						<text>{{i18n.data.data3}}{{dataList.level}}</text>
					</view>
					<view class="level-item"></view>
					<view class="level-item"></view>
				</view>
				<view class="hr"></view>
				<view class="number-wrap">
					<view class="number-item">
						<text class="left">{{i18n.data.lang38}}:</text>
						<text class="right">{{dataList.team?dataList.team.poolTotal:0.00}}</text>
					</view>
					<!-- <view class="number-item">
						<text class="left">{{i18n.data.lang39}}:</text>
						<text class="right">{{dataList.team?dataList.team.last_month:0.00}}</text>
					</view>
					<view class="number-item">
						<text class="left">{{i18n.data.lang40}}:</text>
						<text class="right">{{dataList.team?dataList.team.addPool:0.00}}</text>
					</view> -->
				</view>
				<view class="operation">
					<view class="operation-btn gradient-blue" @tap="getPrize('getWeekPrize')">
						<text :class="{'isget': dataList.award && dataList.award.is_week == 1}">{{i18n.data.lang41_1}}</text>
					</view>
					<view class="operation-btn gradient-violet" @tap="getPrize('getMonthPrize')">
						<text :class="{'isget': dataList.award && dataList.award.is_month == 1}">{{i18n.data.lang41}}</text>
					</view>
					<view class="operation-btn gradient-green" @tap="getPrize('getQuartePrize')">
						<text :class="{'isget': dataList.award && dataList.award.is_quarer == 1}">{{i18n.data.lang42}}</text>
					</view>
				</view>
			</view>
			<swiper class="swiper" autoplay="autoplay">
				<swiper-item v-for="(item, index) in slideList" :key="index">
					<image :src="item.pic" mode="scaleToFill"></image>
				</swiper-item>
			</swiper>
		</view>
		<tabbar :isCurrent="2"></tabbar>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue'
	import Tabbar from '../../components/tabbar.vue'
	export default {
		data() {
			return {
				isBack: false,
				isBg: true,
				dataList: [],
				slideList: [],
			}
		},
		components: {
			HeaderBar,
			Tabbar
		},
		onLoad() {
			this.getData()
			this.getSlideData()
		},
		methods: {
			getData(){
				this.uniRequest({
					url: 'performance',
					method: 'GET'
				}).then(res => {
					this.dataList = res.result
				})
			},
			getSlideData(){
				this.uniRequest({
					url: 'slideshow',
					method: 'GET'
				}).then(res => {
					this.slideList = res.result
				})
			},
			getPrize(url){
				// 获取月奖励
				this.uniRequest({
					url,
					method: 'GET'
				}).then(res => {
					uni.showToast({
						title: res.message,
						icon: 'none',
						success: () => {
							setTimeout(() => {
								this.getData()
							}, 2000)
						}
					})
				})
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
.data{
	min-height: 100%;
	.content {
		width: 100%;
		min-height: 100%;
		box-sizing: border-box;
		padding: 0 30rpx 30rpx;
		background: #292F42 url('../../static/images/bot.png') center top no-repeat;
		background-size: contain;
		.swiper{
			margin-top: 40rpx;
			image{
				width: 100%;
				height: 100%;
			}
		}
		.data-wrap{
			height: 306rpx;
			background:linear-gradient(135deg,rgba(4,190,217,1) 0%,rgba(98,34,225,1) 100%);
			border-radius: 8rpx;
			padding: 0 30rpx;
			overflow: hidden;
			.data-wrap-item{
				height: 103rpx;
				position: relative;
				display: flex;
				align-items: center;
				&::after{
					content: "";
					display: block;
					width: 100%;
					height: 4rpx;
					position: absolute;
					bottom: 0;
					background: url('../../static/images/icon19.png') left bottom repeat-x;
				}
				&:last-child::after{
					display: none;
				}
				image{
					width: 40rpx;
					height: 40rpx;
					margin-right: 16rpx;
				}
				.label{
					font-size: $fontI;
					color: $colorA;
				}
				.count{
					font-size: $fontG;
					color: $colorG;
					margin-left: 30rpx;
				}
			}
		}
		.count-wrap{
			background-color: $colorC;
			border-radius: 8rpx;
			margin-top: 24rpx;
			padding: 32rpx 30rpx;
			.title{
				width: 180rpx;
				height: 38rpx;
				margin: 0 auto;
				background: url('../../static/images/icon23.png') no-repeat;
				background-size: contain;
			}
			.subtitle{
				width: 228rpx;
				height: 44rpx;
				margin: 0 auto;
				background: url('../../static/images/icon23_1.png') no-repeat;
				background-size: contain;
			}
			.count-wrap-list{
				margin-top: 38rpx;
				display: flex;
				justify-content: space-between;
				.count-wrap-item{
					min-height: 138rpx;
					background-color: #1C1F2A;
					border-radius: 8rpx;
					flex: 1;
					text-align: center;
					padding: 22rpx 0;
					box-sizing: border-box;
					margin-left: 20rpx;
					&:first-child{
						margin-left: 0;
					}
					.top{
						font-size: $fontJ;
						color: $colorE;
					}
					.bot{
						font-size: $fontH;
						color: $colorA;
						margin-top: 4rpx;
					}
					.tips{
						font-size: $fontk;
						color: $colorG;
						margin-top: 6rpx;
					}
				}
			}
			.level{
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-top: 8rpx;
				.level-item{
					flex: 1;
					margin-left: 20rpx;
					&:first-child{
						margin-left: 0;
					}
					text{
						display: block;
						font-size: $fontJ;
						color: $colorA;
						text-align: center;
						padding: 10rpx 0;
						background-color: #1C1F2A;
					}
				}
			}
			.hr{
				height: 2rpx;
				background: url('../../static/images/icon19.png') bottom repeat-x;
				margin: 30rpx 0;
			}
			.number-wrap{
				// height: 268rpx;
				background-color: #1C1F2A;
				border-radius: 8rpx;
				padding: 20rpx 40rpx;
				box-sizing: border-box;
				.number-item{
					height: 76rpx;
					display: flex;
					justify-content: space-between;
					align-items: center;
					position: relative;
					&::after{
						content: "";
						display: block;
						height: 2rpx;
						background: url('../../static/images/icon19.png') bottom repeat-x;
						position: absolute;
						bottom: 0;
						left: 0;
						right: 0;
					}
					&:last-child::after{
						display: none;
					}
					.left{
						font-size: $fontI;
						color: $colorA;
					}
					.right{
						font-size: $fontG;
						color: $colorG;
					}
				}
			}
			.operation{
				margin-top: 32rpx;
				display: flex;
				justify-content: space-between;
				align-items: center;
				padding: 0 20rpx;
				.operation-btn{
					flex: 1;
					height: 96rpx;
					line-height: 96rpx;
					text-align: center;
					font-size: $fontG;
					color: $colorA;
					border-radius: 8rpx;
					margin-left: 20rpx;
					&:first-child{
						margin-left: 0;
					}
				}
				.isget{
					position: relative;
					&::after{
						position: absolute;
						content: "";
						display: block;
						width: 12rpx;
						height: 12rpx;
						border-radius: 50%;
						background-color: #FF4436;
						right: -6rpx;
						top: 4rpx;
					}
				}
			}
		}
	}
}
</style>
