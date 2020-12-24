<template>
	<view class="trade">
		<header-bar :isBack="isBack" :isBg="isBg" :title="i18n.header.header29"></header-bar>
		<view class="content">
			<view class="charts-wrap">
				<view class="charts-title">
					<view class="imgbox"></view>
				</view>
				<canvas canvas-id="canvasLineA" id="canvasLineA" class="charts" @touchstart="touchLineA"></canvas>
				<view class="none">暂未开放</view>
			</view>
			<view class="area">
				<view class="area-title"></view>
				<view class="area-content">
					<image src="../../static/images/icon52.png" mode="widthFix"></image>
					<view class="none">暂未开放</view>
				</view>
			</view>
			<view class="game">
				<view class="game-title"></view>
				<view class="game-content">
					<image src="../../static/images/icon56.png" mode="widthFix"></image>
					<view class="none">暂未开放</view>
				</view>
			</view>
		</view>
		<tabbar :isCurrent="3"></tabbar>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue'
	import Tabbar from '../../components/tabbar.vue'
	import uCharts from '../../components/u-charts.min'
	
	var _self;
	var canvaLineA=null;
	export default {
		data() {
			return {
				isBack: false,
				isBg: true,
				cWidth:'',
				cHeight:'',
				pixelRatio:1,
				chartData: {
					categories: [],
					series: []
				}
			}
		},
		components: {
			HeaderBar,
			Tabbar
		},
		onLoad() {
			_self = this;
			this.cWidth=uni.upx2px(690);
			this.cHeight=uni.upx2px(500);
			this.showLineA("canvasLineA", this.chartData)
		},
		methods: {
			showLineA(canvasId,chartData){
				canvaLineA=new uCharts({
					$this:_self,
					canvasId: canvasId,
					type: 'line',
					fontSize:11,
					legend:{show:true},
					dataLabel:false,
					dataPointShape:true,
					background:'#FFFFFF',
					pixelRatio:1,
					categories: chartData.categories,
					series: chartData.series,
					animation: true,
					xAxis: {
						disableGrid: true
					},
					yAxis: {
						gridType: 'dash',
						gridColor: '#ffffff',
						dashLength: 10,
						splitNumber: 2
					},
					width: this.cWidth,
					height: this.cHeight,
					extra: {
						line:{
							type: 'straight'
						}
					}
				});
			},
			touchLineA(e) {
				canvaLineA.showToolTip(e, {
					format: function (item, category) {
						return category + ' ' + item.name + ':' + item.data 
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
.trade{
	.content{
		background: #292F42 url('../../static/images/bot.png') center top no-repeat;
		background-size: contain;
		.swiper1{
			margin: 40rpx 30rpx 30rpx;
			image{
				width: 100%;
				height: 100%;
			}
		}
		.charts-wrap{
			padding: 0 30rpx;
			overflow: hidden;
			position: relative;
			.charts-title{
				background-color: $colorC;
				padding: 20rpx 0;
				border-radius: 8rpx 8rpx 0 0;
				.imgbox{
					width: 186rpx;
					height: 38rpx;
					background: url('../../static/images/icon50.png') no-repeat;
					background-size: contain;
					margin: 0 auto;
				}
			}
			.qiun-charts {
				width: 690upx;
				height: 500upx;
				background-color: $colorC;
			}
			
			.charts {
				width: 690upx;
				height: 500upx;
				background-color: $colorC;
				border-radius: 0 0 8rpx 8rpx;
			}
		}
		.area{
			padding: 32rpx 30rpx 70rpx;
			margin: 24rpx 30rpx 0;
			background-color: $colorC;
			border-radius: 8rpx;
			.area-title{
				width: 314rpx;
				height: 38rpx;
				background: url('../../static/images/icon51.png') no-repeat;
				background-size: contain;
				margin: 0 auto;
			}
			.area-content{
				margin-top: 38rpx;
				position: relative;
				image{
					width: 100%;
				}
			}
		}
		.game{
			padding: 32rpx 30rpx 70rpx;
			margin: 24rpx 30rpx 0;
			background-color: $colorC;
			border-radius: 8rpx;
			.game-title{
				width: 228rpx;
				height: 38rpx;
				background: url('../../static/images/icon55.png') no-repeat;
				background-size: contain;
				margin: 0 auto;
			}
			.game-content{
				margin-top: 38rpx;
				position: relative;
				image{
					width: 100%;
				}
			}
		}
		.none{
			position: absolute;
			left: 50%;
			top: 50%;
			transform: translate(-50%, -50%);
			font-size: $fontH;
			color: $colorE;
		}
	}
}
</style>
