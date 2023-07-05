<template>
	<view class="addform">
		<!-- <uni-section title="我的地址" labelWidth="150rpx" type="line">
			<uni-card v-for="(item, index) in addressList" :key="item.id">
				<radio :value="item.id" />
				<view class="addcontent">
					<text>{{item.username}}</text>
					<view>{{item.telephone}}</view>
					<view>{{item.address}}</view>
				</view>
			</uni-card>
			<button class="btn" @click="addAddress">＋新增地址</button>
		</uni-section> -->
		<view class="addAddress">
			<uni-forms ref="form"  :modelValue="formData" :rules="rules">
				<uni-forms-item label="联系人:" labelWidth="150rpx" name="username">
					<input class="input" v-model="formData.username" type="text" placeholder="请输入联系人"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item>
				<uni-forms-item label="联系电话:" labelWidth="150rpx" name="telephone">
					<input class="input" v-model="formData.telephone" type="text" placeholder="请输入联系电话"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item>
				<uni-forms-item label="省:" labelWidth="150rpx" name="province">
					<input class="input" v-model="formData.province" type="text" placeholder="请输入省"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item>
				<uni-forms-item label="市:" labelWidth="150rpx" name="city">
					<input class="input" v-model="formData.city" type="text" placeholder="请输入市"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item>
				<uni-forms-item label="区/县:" labelWidth="150rpx" name="area">
					<input class="input" v-model="formData.area" type="text" placeholder="请输入区/县"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item>
				<uni-forms-item label="详细地址:" labelWidth="150rpx" name="address">
					<input class="input" v-model="formData.address" type="text" placeholder="详细到门牌号"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item>
			</uni-forms>
		</view>
		<button class="addbtn" @click="addAddressSub">添加地址</button>
	</view>
</template>

<script>
	// import cityData from '/n'
	export default {
		data() {
			return {
				addressList: '',
				userInfo:'',
				// 表单数据
				formData:{
					username:'',
					telephone:'',
					province:'',
					city:'',
					area:'',
					address:'',
				}
			};
		},
		onLoad() {
			this.addressList = uni.getStorageSync('addressList')
			this.userInfo=uni.getStorageSync('userInfo').id
		},
		methods: {
			addAddressSub() {
				console.log(this.formData.address)
				uni.request({
					url:'http://localhost:8002/address/saveOrUpdate',
					data:{
						id:'',
						username:this.formData.username,
						telephone:this.formData.telephone,
						province:this.formData.province,
						city:this.formData.city,
						area:this.formData.area,
						address:this.formData.address,
						isDefault:0,
						userId:this.userInfo
					},
					method:'POST',
					success:(res)=>{
						if(res.data.status==200){
							uni.showToast({
								title:"添加成功！"
							},4000)
							// uni.navigateTo({
							// 	url:"/pages/orderList/orderList"
							// },4000)
							// console.log(res)
							
						}
						
					}
				})
			}
		},
	};
</script>
<style lang="scss">
	.addform{
		padding: 30rpx 40rpx;
		.addAddress{
			
			.input {
				width: 400rpx;
				height: 40rpx;
				margin-top: 20rpx;
				border: 1rpx solid #dadbde;
				padding: 12rpx 18rpx;
				border-radius: 8rpx;
				font-size: 28rpx;
				color: rgb(48, 49, 51);
			}
			
		}
	}
	.addbtn {
		margin-bottom: 30rpx;
		width: 100%;
		height: 100rpx;
		background-color: #FD5554;
		color: #333;
		text-align: center;
		border-radius: 8rpx;
	}
</style>
