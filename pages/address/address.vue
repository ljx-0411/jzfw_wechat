<template>
	<view>
		<uni-section title="我的地址" labelWidth="150rpx" type="line">
			<uni-card v-for="(item, index) in addressList" :key="item.id">
				<view class="addcontent">
					<text>{{item.username}}</text>
					<view>{{item.telephone}}</view>
					<text>{{item.province}}</text>
					<text>{{item.city}}</text>
					<text>{{item.area}}</text>
					<view>{{item.address}}</view>
				</view>
				<button @click="deladdress(item.id)">删除地址</button>
			</uni-card>
			<navigator class='row' url='../addAddress/addAddress'>
				<button class="btn">＋新增地址</button>
			</navigator>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				addressList: '',
				userInfo:'',
				token:''
			}
		},
		onLoad() {
			// this.addressList = uni.getStorageSync('addressList')
		},
		onShow() {
			this.getAddress()

		},
		methods: {
			getAddress() {
				this.token=uni.getStorageSync('token');
				uni.request({
					url:'http://localhost:8002/user/info',
					data:{
						token:this.token
					},success:(res)=>{
						this.userInfo=res.data.data
						uni.setStorageSync('userInfo',this.userInfo)
						console.log(res.data.data)
					}
				})
				this.userInfo = uni.getStorageSync('userInfo');
				// console.log(this.userInfo.id)
				uni.request({
					url: 'http://localhost:8002/address/pageQuery',
					data: {
						page: 1,
						pageSize: 9,
						userId: this.userInfo.id
					},
					success: (res) => {
						console.log(res.data.data)
						this.addressList = res.data.data.list;
						uni.setStorageSync('addressList', this.addressList)
						console.log(this.addressList)

					}
				})
			},
			deladdress(id){
				uni.request({
					url:'http://localhost:8002/address/deleteById',
					data:{
						id:id
					},
					success: (res) => {
						console.log(res)
						if(res.data.status==200){
							uni.showToast({
								title: '删除成功！',
								icon:'success'
							});
							this.getAddress()
						}
						
					}
				})
			}
		}
	}
</script>

<style>
	.btn {
		margin-top: 30rpx;
		width: 100%;
		height: 100rpx;
		background-color: #ffffff;
		color: #333;
		text-align: center;
		border-radius: 8rpx;
	}
</style>
