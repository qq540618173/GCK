<template>
	<view class="miningdetails">
		<header-bar :title="i18n.header.header9"></header-bar>
		<view class="container">
			<view class="detail-wrap">
				<view class="status-title">
					<text>{{i18n.index.mining6}}</text>
					<text @tap="gotoPage(`machine?type=2`)" style="color: #33D38A" v-if="currentData.status == 2">{{i18n.index.mining8}}</text>
				</view>
				<view class="status-info" :class="'status' + currentData.status">
					<text>{{i18n.index['status' + currentData.status]}}</text>
				</view>
			</view>
			<view class="detail-wrap">
				<view class="top flex">
					<text>{{i18n.index.mining7}}</text>
					<text>{{i18n.index['mining' + currentData.combo_id]}}</text>
				</view>
				<view class="bot flex">
					<uni-countdown :show-day="false" :hour="0" :minute="minute" :second="second" @timeup="getResult(true)"></uni-countdown>
					<text>{{currentData.money}}GCK</text>
				</view>
			</view>
			<view class="detail-wrap">
				<view class="title"></view>
				<view class="canvas-wrap">
					<canvas style="width: 630rpx; height: 630rpx;" canvas-id="gameCanvas"></canvas>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import HeaderBar from '../../components/header-bar.vue';
	import uniCountdown from '@/components/uni-countdown/uni-countdown.vue';
	var T = null
	export default {
		data() {
			return {
				currentData: {},
				minute: 0,
				second: 0,
				order: '',
				seat: '',    //座位号
			}
		},
		components:{
			HeaderBar,
			uniCountdown
		},
		onLoad(option) {
			this.order = option.order
			this.getData(option.order)
			this.getResult(false)
			T = setInterval(() => {
				if(this.minute == 0 && this.second == 0){
					this.getData(option.order)
					clearInterval(T)
				}else{
					this.getData(option.order)
				}
			}, 60000)
		},
		onUnload() {
			clearInterval(T)
		},
		methods: {
			getData(order){
				this.uniRequest({
					url: 'luckyInfo',
					method: 'GET',
					data: {
						order
					}
				}).then(res => {
					let data = res.result
					this.currentData = data
					this.minute = Math.floor(data.countdown/60)
					this.second = data.countdown%60
					this.seat = data.number
					if(data.lotteryNumber){
						this.drawContext(data.lotteryNumber-1, data.number, false)
					}else{
						this.drawContext(0, data.number, false)
					}
				})
			},
			getResult(onOff){
				let { order } = this
				this.uniRequest({
					url: 'drawLottery',
					method: 'GET',
					data: {
						order
					}
				}).then(res => {
					let number = res.result.number
					if(number){
						this.currentData = res.result
					}
					this.drawContext(number-1, this.seat, onOff)
				})
			},
			gotoPage(url){
				uni.navigateTo({
				    url
				});
			},
			drawContext(current, myCurrent, rotates){               //current:当前指向座位号，myCurrent:我的座位号，rotates:是否旋转
				let cW = uni.upx2px(630)        //画布宽度
				let cH = uni.upx2px(630)        //画布高度
				let cR = uni.upx2px(630)/2      //画布半径
				let cT = uni.upx2px(60)
				let aW = uni.upx2px(80)         //指针宽度
				let aH = uni.upx2px(123)        //指针高度
				let aW1 = -uni.upx2px(80)/2     //指针X偏移量
				let aH1 = -uni.upx2px(80)       //指针y偏移量
				let txt1 = uni.upx2px(36)       //文字大小1
				let txt2 = uni.upx2px(20)       //文字大小2
				let cr = uni.upx2px(28)         //小圆半径
				let timer = null
				const innerAudioContext = uni.createInnerAudioContext();
				innerAudioContext.loop = true;
				innerAudioContext.startTime = 1
				innerAudioContext.src = './../../static/bgm.wav';
				let context = uni.createCanvasContext('gameCanvas')    //定义画布
				if(rotates){
					let oIndex = 0;
					innerAudioContext.play()
					innerAudioContext.onPlay(() => {
						timer = setInterval(() => {
							oIndex++
							drawBox(oIndex, myCurrent)
							if( oIndex == 160+current ){
								clearInterval(timer)
								drawBox(oIndex, myCurrent)
								innerAudioContext.destroy()
								this.getData(this.order)
							}
						}, 37)
					})
				} else {
					drawBox(current, myCurrent)
				}
				
				function drawBox(current, myCurrent){
					drawBackGround()
					drawArc(current, myCurrent)
					drawArrow(current)
					drawTxet(current)
				}
				
				function drawBackGround(){
					context.save()
					context.drawImage('../../static/images/lunpan.png', 0, 0, cW, cH)
					context.draw()
					context.restore()
				}
				function drawArc(current, myCurrent){
					let num = [1, 2, 3, 4, 5, 6, 7, 8]   //座位数
					num[myCurrent-1] = '我'
					let newCurrent = current%num.length  //当前指针指向座位
					context.save()
					context.translate(cR, cR)
					//座位号背景圆
					for(var i=0; i<num.length; i++){
						let rad = 2 * Math.PI / 8 * i - (0.5 * Math.PI)
						let x = Math.cos(rad) * (cR - cT)
						let y = Math.sin(rad) * (cR - cT)
						context.beginPath()
						if(i%8 == myCurrent-1){
							context.setFillStyle('#33D38A')
							context.arc(x, y, cr, 0 , 2 * Math.PI, false)
						} else {
							context.setFillStyle('#222636')
							context.arc(x, y, cr, 0 , 2 * Math.PI, false)
						}
						context.fill()
					}
					//座位号
					context.setFontSize(txt1)
					context.setTextAlign('center')
					context.setTextBaseline('middle')
					num.forEach((item, index) => {
						let rad = 2 * Math.PI / 8 * index - (0.5 * Math.PI)
						let x = Math.cos(rad) * (cR - cT)
						let y = Math.sin(rad) * (cR - cT)
						
						if (index%8 == myCurrent-1){
							context.setFillStyle('#FFFFFF')
							context.fillText(item, x, y)
						} else {
							context.setFillStyle('#5522FA')
							context.fillText(item, x, y)
						}
					})
					context.draw(true)
					context.restore()
				}
				
				function drawArrow(current){
					let aRad = 2 * Math.PI / 8 * current
					context.save()
					context.translate(cR, cR)
					context.rotate(aRad)
					context.drawImage('../../static/images/arrow.png', aW1, aH1, aW, aH)
					context.draw(true)
					context.restore()
				}
				
				function drawTxet(current){
					let t1 = null
					if(current>0){
						t1 = '已经'
					} else{
						t1 = '暂未'
					}
					context.save()
					context.translate(cR, cR)
					context.setFontSize(txt2)
					context.setFillStyle('#FFFFFF')
					context.fillText(t1, -txt2, -2)
					context.fillText('开奖', -txt2, txt2)
					context.draw(true)
					context.restore()
				}
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
.miningdetails{
	.container{
		.detail-wrap{
			background-color: $colorC;
			padding: 30rpx;
			border-radius: 8rpx;
			margin-bottom: 24rpx;
			.status-title{
				font-size: $fontJ;
				color: $colorB;
				display: flex;
				justify-content: space-between;
				align-items: center;
			}
			.status-info{
				font-size: $fontH;
				margin-top: 12rpx;
				&.status0{
					color: $colorH;
				}
				&.status1{
					color: #FF4436;
				}
				&.status2{
					color: $colorG;
				}
			}
			.flex{
				display: flex;
				justify-content: space-between;
				align-items: center;
			}
			.top{
				font-size: $fontJ;
				color: $colorB;
			}
			.bot{
				margin-top: 12rpx;
				font-size: $fontD;
				text{
					&:first-child{
						color: $colorA;
					}
					&:last-child{
						color: $colorG;
					}
				}
			}
			.title{
				width: 176rpx;
				height: 38rpx;
				margin: 14rpx auto 0;
				background: url('../../static/images/icon53.png') no-repeat;
				background-size: contain;
			}
		}
	}
}
</style>
