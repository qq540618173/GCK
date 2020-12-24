<template>
	<view class="mining">
		<header-bar :title="i18n.header.header9"></header-bar>
		<view class="container">
			<view class="mining-item" :class="'item' + index" v-for="(item, index) in miningList" :key="index" @tap="showModal(item.id)">
				<view class="left">
					<view class="text text1">
						<text>{{i18n.index['mining'+item.id]}}</text>
					</view>
					<view class="text text2">
						<text>{{item.price}}GCK</text>
					</view>
					<view class="text text3">
						<text>{{i18n.index.mining15}}</text>
					</view>
				</view>
				<view class="right" :class="'right' + index"></view>
			</view>
			<swiper class="swiper swiper1" autoplay="autoplay">
				<swiper-item v-for="(item, index) in slideList" :key="index">
					<image :src="item.pic" mode="scaleToFill"></image>
				</swiper-item>
			</swiper>
			<view class="modal" v-show="showBuy">
				<view class="modal-content">
					<view class="form-item">
						<input type="text" v-model="paypass" :password="active" :placeholder="i18n.withdrawal.lang20" />
						<view class="password" :class="{'active': !active}" @tap="addClass('active')"></view>
					</view>
					<view class="modal-btn">
						<view class="cancel gradient-green" @tap="cancel">
							<text>{{i18n.index.mining4}}</text>
						</view>
						<view class="sure gradient-blue" @tap="sure">
							<text>{{i18n.index.mining5}}</text>
						</view>
					</view>
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
				active: true,
				miningList: [],
				slideList: [],
				showBuy: false,
				id: '',
				paypass: ''
			}
		},
		components:{
			HeaderBar
		},
		onLoad() {
			this.getData()
			this.getSlideData()
		},
		methods: {
			getData(){
				this.uniRequest({
					url: 'combo',
					method: 'GET'
				}).then(res => {
					this.miningList = res.result
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
			showModal(id){
				this.id = id
				this.uniRequest({
					url: 'isAccess',
					method: 'GET',
					data: {
						combo_id: id
					}
				}).then(res => {
					let { order } = res.result
					if(order){
						uni.navigateTo({
							url: `miningdetails?order=${order}`
						})
					} else {
						this.showBuy = true
					}
				})
			},
			addClass(classname){
				this[classname] = !this[classname]
			},
			cancel(){
				this.showBuy = false
				this.paypass = ''
			},
			sure(){
				let { id, paypass } = this
				this.uniRequest({
					url: 'lucky',
					data: {
						id,
						paypass
					}
				}).then(res => {
					this.cancel()
					uni.navigateTo({
					    url: `miningdetails?order=${res.result.order}`
					});
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
.mining{
	.swiper1{
		margin-top: 40rpx;
		image{
			width: 100%;
			height: 100%;
		}
	}
	.mining-item{
		border-radius: 8rpx;
		padding: 24rpx 54rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		// position: relative;
		margin-bottom: 24rpx;
		&.item0{
			background:linear-gradient(180deg,rgba(190,167,240,1) 0%,rgba(123,105,217,1) 100%);
			box-shadow:0px 4px 4px 0px rgba(0,0,0,0.2),0px 1px 1px 0px rgba(255,255,255,0.5),0px -1px 1px 0px rgba(54,40,127,1);
		}
		&.item1{
			background:linear-gradient(180deg,rgba(119,207,244,1) 0%,rgba(68,99,205,1) 100%);
			box-shadow:0px 4px 4px 0px rgba(0,0,0,0.2),0px 1px 1px 0px rgba(255,255,255,0.5),0px -1px 1px 0px rgba(40,85,127,1);
		}
		&.item2{
			background:linear-gradient(180deg,rgba(49,208,140,1) 0%,rgba(32,176,168,1) 100%);
			box-shadow:0px 4px 4px 0px rgba(0,0,0,0.2),0px 1px 1px 0px rgba(255,255,255,0.5),0px -1px 1px 0px rgba(20,121,115,1);
		}
		.left{
			.text{
				color: $colorA;
			}
			.text1{
				font-size: $fontA;
			}
			.text2{
				font-size: $fontH;
				margin-top: 20rpx;
				background: rgba(0,0,0,.3);
				padding: 4rpx 10rpx;
				border-radius: 34rpx;
			}
			.text3{
				font-size: $fontI;
				margin-top: 40rpx;
				text{
					position: relative;
					&::after{
						content: "";
						display: block;
						width: 40rpx;
						height: 40rpx;
						background: url('../../static/images/icon18.png') no-repeat;
						background-size: contain;
						position: absolute;
						right: -40rpx;
						top: 50%;
						margin-top: -18rpx;
					}
				}
			}
		}
		.right{
			width: 272rpx;
			height: 256rpx;
			&.right0{
				background: url('../../static/images/lunpan1.png') no-repeat;
				background-size: contain;
			}
			&.right1{
				background: url('../../static/images/lunpan2.png') no-repeat;
				background-size: contain;
			}
			&.right2{
				background: url('../../static/images/lunpan3.png') no-repeat;
				background-size: contain;
			}
		}
	}
	.modal{
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0,0,0,0.5);
		z-index: 998;
		.modal-content{
			position: absolute;
			left: 30rpx;
			right: 30rpx;
			top: 50%;
			transform: translateY(-50%);
			background-color: $colorC;
			border-radius: 8rpx;
			padding: 40rpx 30rpx;
			.modal-btn{
				display: flex;
				justify-content: space-between;
				align-items: center;
				margin-top: 40rpx;
				.cancel{
					margin-right: 40rpx;
				}
				.cancel,
				.sure{
					line-height: 80rpx;
					border-radius: 8rpx;
					text-align: center;
					font-size: $fontG;
					color: $colorA;
					flex: 1;
				}
			}
		}
	}
}
</style>
