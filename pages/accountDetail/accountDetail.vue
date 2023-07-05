<template>
	<view>
		<uni-card  v-for="item in accList" :key="item.id">
			<view class="time">{{item.transferTime|formatDate}}</view>
			<view class="content">
				<text class="type">{{item.type}}</text>
				<text class="money">+{{item.transferMoney}}</text>
			</view>
				
			</uni-card>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				accList:[],
				userInfo:''
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
			this.userInfo=uni.getStorageSync('userInfo')
			this.getAccount()
		},
		onShow() {
			this.getAccount()
		},
		methods: {
			getAccount(){
				uni.request({
					url:'http://localhost:8002/account/pageQueryEmployeeAccount',
					data:{
						page:1,
						pageSize:99,
						userId:this.userInfo.id
					},
					success: (res) => {
						this.accList=res.data.data.list
						console.log(this.accList)
					}
				})
			}
		}
	}
</script>

<style lang="scss">
.uni-card{
	height: 110rpx;
	.time{
		// margin-left: 500rpx;
	}
	.content{
		.money{
			position: absolute;
			right: 10rpx;
			color:#FF201C;
		}
		.type{
			color: #333;
		}
	}
	
	
}
</style>
