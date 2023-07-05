<template>
	<view>
		<uni-card  v-for="item in orderList" :key="item.id">
			<view class="time">{{item.orderTime|formatDate}}</view>
			<view class="content">
				<view class="left">
					<img :src="item.orderLines[0].product.photo" alt="">
				</view>
				<view class="right">
					<text class="name">{{item.orderLines[0].product.name}}</text>
					<view class="total">{{item.total}}￥</view>
					<uni-tag inverted :text="item.status" type="primary" ></uni-tag>
					<navigator :url="`../orderDetail/orderDetail?id=${item.id}`">
						<text class="detail" >订单详情></text>
					</navigator>
					
				</view>
				<button class="del"  v-if="item.status=='已完成' || item.status== '待派单'" @click="delOrder(item.id)">删除订单</button>
			</view>
		</uni-card>
	</view>
</template>

<script>
	// import {formatTimestamp} from '@/utils/dateTime.js';
	export default {
		data() {
			return {
				// 订单数组
				orderList:[],
				userInfo:'',
				// formatTimestamp:formatTimestamp()
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
			
			delOrder(id){
				uni.request({
					url:'http://localhost:8002/order/deleteById',
					data:{
						id:id
					},
					success:(res)=>{
						if(res.status==200){
								uni.showToast({
									title:'删除成功！',
									// duration:3000
								},3000)
							
							
						}
						this.getOrder()
						// console.log(res)
					}
				})
			},
			getOrder(){
				this.userInfo=uni.getStorageSync("userInfo").id
				uni.request({
					url:'http://localhost:8002/order/pageQuery',
					data:{
						page:1,
						pageSize:99,
						customerId:this.userInfo
					},
					success:(res)=>{
						this.orderList=res.data.data.list
						console.log(this.orderList)
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	 .uni-card{
		 height: 350rpx;
		 margin-top: 10rpx;
		 padding: 10rpx 10rpx ;
		 position: relative;
		 .content{
			  display: flex;
			  margin-top: 10rpx;
			  .left{
				  img{
				  				 width: 200rpx;
				  				 height: 200rpx;
				  }
			  }
			 .right{
				margin-left: 10rpx;
				.name{
					color:#333;
					font-size: 30rpx;
				}
				.total{
					position: absolute;
					top: 60rpx;
					right: 30rpx;
					font-weight: bold;
				}
				.uni-tag{
					display: block;
					position: absolute;
					left: 250rpx;
					bottom: 75rpx;
					width: 75rpx;
				}
					
				.detail{
					position: absolute;
					bottom: 75rpx;
					right: 30rpx;
					color:#808080;
				}
			 }
			 .del{
				 margin-top: 5rpx;
				 position: absolute;
				 height: 60rpx;
				 width: 200rpx;
				 bottom: 10rpx;
				 right: 10rpx;
				 color:#333;
				 font-size: 28rpx;
			 }
			 
		 }
		 .time{
			 // position: absolute;
			 // bottom: 5rpx;
			 // right: 30rpx;
		 }
	 }
</style>
