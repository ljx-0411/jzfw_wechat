<template>
	<view>

		<view class="title">{{detail.title}}</view>
		<view class="time">
			{{detail.publishTime|formatDate}}
		</view>
		<img class="img" :src="detail.cover" alt="">
		<view class="content" v-html="detail.content">
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: '',
				detail: '',

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
		onLoad(option) {
			//路由传参过来点击订单的id
			this.id = option.id
			// console.log(option.id)
			this.getDetailById()
		},
		methods: {
			getDetailById() {
				console.log(this.id)
				uni.request({
					url: `http://localhost:8002/article/findById?id=${this.id}`,
					// data:{
					// 	id:this.id
					// },
					success: (res) => {
						this.detail = res.data.data
						console.log(this.detail)
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.title {
		margin-top: 10rpx;
		font-size: 40rpx;
		text-align: center;
		font-weight: bold;
	}

	.time {
		text-align: center;
		margin-left: 300rpx;
		font-size: 30rpx;
		color: #c1c1c1;
		margin-top: 30rpx;
	}

	.img {
		margin-top: 20rpx;
		margin-left:  200rpx;
		width: 400rpx;
		height: 300rpx;
	}
	.content{
		margin-top: 20rpx;
		padding:20rpx ;
		text-indent: 2em;
		line-height: 50rpx;
		font-size: 28rpx;
	}
</style>
