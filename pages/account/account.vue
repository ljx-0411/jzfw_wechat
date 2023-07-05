<template>
	<view >
		<view class="box">
			<text>充值金额：</text>
			<input class="input" type="text" v-model="money" >
			<button class="button" @click="recharge">确认充值</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				money:''
			}
		},
		onLoad() {
			
		},
		methods: {
			recharge(){
				uni.request({
					url:'http://localhost:8002/account/recharge',
					data:{
						customerId:uni.getStorageSync('userInfo').id,
						money:this.money
					},
					success:(res)=> {
						// console.log(res.data.status)
						if(res.data.status==200){
							uni.showToast({
								title:'充值成功！',
								icon:'success',
								duration: 2000
							})
							setTimeout(()=>{
								uni.navigateBack({
									delta:1,
									
								})
							},2000)
						}
						
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.box{
		padding: 200rpx 100rpx;
		.input {
			margin-top: 20rpx;
				width: 400rpx;
				height: 40rpx;
				margin-top: 20rpx;
				border: 1rpx solid #dadbde;
				padding: 12rpx 18rpx;
				border-radius: 8rpx;
				font-size: 28rpx;
				color: rgb(48, 49, 51);
			}
		.button{
			margin-top: 200rpx;
			width: 100%;
			height: 100rpx;
				background-color: #FD5554;
				color: #fff;
				text-align: center;
				border-radius: 8rpx;
		}
	}
	
</style>
