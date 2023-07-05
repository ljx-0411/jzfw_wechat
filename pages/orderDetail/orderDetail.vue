<template>
	<view>
		<uni-card :title="title" :sub-title="titleSmall" :extra="extra" :thumbnail="thumbnail">
			<view class="text">下单时间：{{detailList.orderTime|formatDate}}</view>
			<view  class="text">服务时间：{{detailList.serviceTime|formatDate}}</view>
			<view  class="text">下单人：{{detailList.customer.realname}}</view>
			<view  class="text">电话号码：{{detailList.customer.telephone}}</view>
			<view  class="text">下单地址：{{detailList.address.province}}{{detailList.address.city}}{{detailList.address.area}}{{detailList.address.address}}</view>
			<view  class="text">联系人：{{detailList.address.username}}</view>
			<view  class="text">联系电话:{{detailList.address.telephone}}</view>
		</uni-card>
		<button class="save" v-if="userInfo.roles[0].id==1 && detailList.status=='已接单' || detailList.status=='待服务' " @click="saveOrder">结束订单</button>
		<button class="save" v-if="userInfo.roles[0].id==6  && detailList.status=='待确认'" @click="confirmOrder">确认订单</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id:'',
				detailList:[],
				title:'',
				extra:'',
				thumbnail:'',
				titleSmall:'',
				userInfo:''
			}
		},
		onLoad(option) {
			//路由传参过来点击订单的id
			this.id = option.id
			// console.log(option.id)
			this.orderDetail()
			this.userInfo=uni.getStorageSync('userInfo')
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
		methods: {
			// 订单详情获取
			orderDetail() {
				uni.request({
					url: 'http://localhost:8002/order/findById',
					data: {
						id: this.id
					},
					success: (res) => {
						this.detailList = res.data.data
						this.title=this.detailList.orderLines[0].product.name;
						this.extra='￥'+this.detailList.orderLines[0].product.price;
						this.titleSmall=this.detailList.status;
						this.thumbnail=this.detailList.orderLines[0].product.photo;
						console.log(res.data.data)
					}
				})
			},
			confirmOrder(){
				uni.request({
						url: `http://localhost:8002/order/confirmOrder`,
						data: {
							orderId: this.id
						},
						success: (res) => {
							console.log(res)
							if(res.data.status==200){
								uni.showToast({
									title: '已确认！',
									icon:'success'
								});
								this.orderDetail()
							}
						
						}
					})
			},
			saveOrder(){
				uni.request({
						url: `http://localhost:8002/order/serviceCompleteOrder`,
						data: {
							orderId: this.id
						},
						success: (res) => {
							console.log(res)
							if(res.data.status==200){
								uni.showToast({
									title: '已结束！',
									icon:'success'
								});
								this.orderDetail()
							}
						
						}
					})
				},
			}
		
	}
</script>

<style lang="scss">
	.text{
		line-height: 50rpx;
		
	}
	.save {
		margin-top: 50rpx;
		margin-left: 500rpx;
	
		height: 80rpx;
		width: 200rpx;
		
		color: #333;
		font-size: 28rpx;
	}

</style>
