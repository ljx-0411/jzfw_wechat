<template>
	<view>
		<uni-card v-for="item in orderList" :key="item.id">
			<view class="time">{{item.orderTime|formatDate}}</view>
			<view class="content">
				<view class="left">
					<img :src="item.orderLines[0].product.photo" alt="">
				</view>
				<view class="right">
					<text class="name">{{item.orderLines[0].product.name}}</text>
					<view class="total">{{item.total}}￥</view>
					<uni-tag inverted :text="item.status" type="primary"></uni-tag>
					<navigator :url="`../orderDetail/orderDetail?id=${item.id}`">
						<text class="detail">订单详情></text>
					</navigator>

				</view>
				<button class="acc" v-if="item.status=='待接单'" @click="acceptOrder(item.id)">接单</button>
				<button class="del"  v-if="item.status=='待接单'" @click="rejectOrder(item.id)">拒绝订单</button>
			</view>
		</uni-card>

<view class="box">
	<view class="user" @click="goUser">
		<img class='img' src="/static/img/index/menuicon1.png" alt="">
	</view>
</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: '',
				orderList: ''
			}
		},
		filters: {
			formatDate(timestamp) {
				const date = new Date(timestamp);
				const year = date.getFullYear();
				const month = String(date.getMonth() + 1).padStart(2, '0');
				const day = String(date.getDate()).padStart(2, '0');
				const hours = String(date.getHours()).padStart(2, '0');
				const minutes = String(date.getMinutes()).padStart(2, '0');
				return `${year}-${month}-${day} ${hours}:${minutes}`;
			}
		},
		onLoad() {
			this.getOrder()
		},
		onShow() {
			this.getOrder()
		},
		methods: {
			getOrder() {
				this.userInfo = uni.getStorageSync("userInfo")
				uni.request({
					url: 'http://localhost:8002/order/pageQuery',
					data: {
						page: 1,
						pageSize: 99,
						employeeId: this.userInfo.id
					},
					success: (res) => {
						this.orderList = res.data.data.list
						console.log(this.orderList)
					}
				})
			},
			acceptOrder(id) {
				uni.request({
					url: 'http://localhost:8002/order/takeOrder',
					data: {
						orderId: id
					},
					success: (res) => {
						console.log(res)
						if (res.data.status == 200) {
							uni.showToast({
								title: '已接单！',
								icon: 'success'
							});
							this.getOrder()
						}
						
					}
				})
			},
			rejectOrder(id) {
				uni.request({
					url: 'http://localhost:8002/order/rejectOrder',
					data: {
						orderId: id
					},
					success: (res) => {
						// console.lopuser
						if (res.data.status == 200) {
							uni.showToast({
								title: '已拒绝！',
								icon: 'success'
							});
							this.getOrder()
						}
					}
				})
			},
			goUser() {
				uni.redirectTo({
					url: '/pages/empuser/empuser'
				})
			}
		}
	}
</script>

<style lang="scss">
	.uni-card {
		height: 350rpx;
		margin-top: 10rpx;
		padding: 10rpx 10rpx;
		position: relative;

		.content {
			display: flex;
			margin-top: 10rpx;

			.left {
				img {
					width: 200rpx;
					height: 200rpx;
				}
			}

			.right {
				margin-left: 10rpx;

				.name {
					color: #333;
					font-size: 30rpx;
				}

				.total {
					position: absolute;
					top: 60rpx;
					right: 30rpx;
					font-weight: bold;
				}

				.uni-tag {
					display: block;
					position: absolute;
					left: 250rpx;
					bottom: 75rpx;
					width: 75rpx;
				}

				.detail {
					position: absolute;
					bottom: 75rpx;
					right: 30rpx;
					color: #808080;
				}
			}

			.acc {
				margin-top: 5rpx;
				position: absolute;
				height: 60rpx;
				width: 200rpx;
				bottom: 10rpx;
				right: 250rpx;
				color: #333;
				font-size: 28rpx;
			}

			.del {
				margin-top: 5rpx;
				position: absolute;
				height: 60rpx;
				width: 200rpx;
				bottom: 10rpx;
				right: 10rpx;
				color: #333;
				font-size: 28rpx;
			}

		}

		.time {
			// position: absolute;
			// bottom: 5rpx;
			// right: 30rpx;
		}

	}
.box{
	
	.user {
		width: 100rpx;
		height: 100rpx;
		position: fixed;
		top: 1000rpx;
		right: 10rpx;
		background-color: #FD5554;
		border-radius: 50rpx;
		color: #fff;
		text-align: center;
		line-height: 90rpx;
	
		.img {
			width: 100rpx;
			height: 100rpx;
		}
	}
}
	
</style>
