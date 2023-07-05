<template>
	<view>
		<view class="proDatail">
			<view class="photo">
				<image :src="prodatails.photo" mode="widthFix"></image>
			</view>
			<view class="content">
				<view class="name">
					<text >{{prodatails.name}}</text>
				</view>
				<view class="descrip">
					<text >{{prodatails.description}}</text>
				</view>
				<view class="price">
					价格：<text >{{prodatails.price}}￥</text>
				</view>
				<view class="cate">
					<text >所属分类：{{prodatails.category.name}}</text>
					<img :src="prodatails.category.icon" alt="">
				</view>
				
			</view>
			
		</view>
		<!-- 底部预约订单导航 -->
		<view class="uni-container">
					<view class="goods-carts">
						<uni-goods-nav :fill="true" :options="options" :button-group="customButtonGroup1" 
										@buttonClick="submitOrder" style="margin-top: 20px;" />
					</view>
			</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 产品详情
				prodatails:null,
				order:{},
				userInfo:null,
				options: [{
									icon: 'chat',
									text: '客服'
								}],
								customButtonGroup1: [{
													text: '订单预约',
													backgroundColor: 'linear-gradient(90deg, #FFB172, #EF1224)',
													color: '#fff'
												}]
							
			}
		},
		onLoad(){
			this.prodatails=uni.getStorageSync("prodatails")[0];
			this.userInfo=uni.getStorageSync('userInfo')
			console.log(this.userInfo)
			// console.log(this.prodatails)
		},
		methods: {
			
				submitOrder() {
					uni.navigateTo({
						url: '/pages/orderList/orderList'
					}, 2000)
					
							
						},
						
		}
	}
</script>

<style lang="scss">
.proDatail{
	color: #333;
	.photo{
		image{
			width: 100%;
		}
	}
	.content{
		margin-top: 20rpx;
		padding: 5rpx 10rpx;
		.name{
			font-size: 40rpx
			
		}
		.price{
			text{
				color: #FD5554;
			}
		}
		.descrip{
			margin: 10rpx 0;
			text-indent: 2em;
			font-size: 28rpx;
		}
		.cate{
			text-align: right;
			margin-bottom: 100rpx;
			img{
				margin-top: 10rpx;
				margin-left: 10rpx;
				width: 40rpx; 
				height: 40rpx;
			}
		}
	}
}
.goods-carts {
		display: flex;
		flex-direction: column;
		position: fixed;
		left: 0;
		right: 0;
		left: var(--window-left);
		right: var(--window-right);
		bottom: 0;
	}
</style>
