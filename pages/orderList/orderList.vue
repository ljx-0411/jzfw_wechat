<template>
	<view>
		<view class="forms">
			<uni-forms ref="form" :modelValue="formData" :rules="rules">
				<uni-forms-item label="数量:" labelWidth="150rpx" name="number">
					<uni-number-box :min="1" :max="99" @change="changeNum" />
				</uni-forms-item>
				<!-- <uni-forms-item label="联系人:" labelWidth="150rpx" name="name">
					<input class="input" v-model="formData.email" type="text" placeholder="请输入联系人"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item> -->
				<!-- <uni-forms-item label="联系电话:" labelWidth="150rpx" name="telephone">
					<input class="input" v-model="formData.email" type="text" placeholder="请输入联系电话"
						@input="binddata('email',$event.detail.value)" />
				</uni-forms-item> -->

				<uni-forms-item label="服务时间:" labelWidth="150rpx" name="time">
					<view class="example-body">
						<uni-datetime-picker returnType="timestamp" v-model="single" @change="changeLog($event)" />
					</view>
				</uni-forms-item>
				<uni-section title="我的地址" labelWidth="150rpx" type="line">
					<radio-group @change="addressChange">
						<label v-for="(item, index) in addressList" :key="item.id">
							<uni-card>
								<radio :value="item.id" />
								<view class="addcontent">
									<text>{{item.username}}</text>
									<view>{{item.telephone}}</view>
									<text>{{item.province}}{{item.city}}{{item.area}}{{item.address}}</text>
								</view>
							</uni-card>
						</label>
					</radio-group>
					<button class="btn" @click="addAddressbtn">＋新增地址</button>
				</uni-section>
				
			</uni-forms>


		</view>
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
				// 当前登录用户信息
				userInfo: null,
				// 当前用户地址信息
				addressList: [],
				adddressId: '',
				number: 1,
				prodatails: '',
				options: '43',
				customButtonGroup1: [{
					text: '提交订单',
					backgroundColor: 'linear-gradient(90deg, #FFB172, #EF1224)',
					color: '#fff'
				}],
				// 选择返回的时间字符串
				single: '',
				start: Date.now(),
				end: Date.now() + 1000000000,
				// 表单验证
				// rules: {
				// 	// 对name字段进行必填验证
				// 	telephone: {
				// 		rules: [{
				// 				required: true,
				// 				errorMessage: '请输入号码',
				// 			},
				// 			{
				// 				minLength: 11,
				// 				maxLength: 11,
				// 				errorMessage: '请输入正确格式的号码！',
				// 			}
				// 		]
				// 	},

				// }
			}
		},
		onLoad() {
			this.getAddress()
		},
		onShow() {
		  // 在页面显示时执行刷新操作
		  this.getAddress();
		},
		
		methods: {
			
			// 选择地址信息返回id
			addressChange(e) {
				this.addressId = e.detail.value
				// console.log(e.detail)
				console.log(this.addressId)
			},
			// 计步器点击返回事件
			changeNum(value) {
				this.number = value
				console.log('返回数值：', value);
			},
			changeLog(e) {
				// 单选返回日期字符串，
				this.single = e
				console.log('change事件:', this.single = e);
			},
			// 根据当前登录用户获取地址信息
			getAddress() {
				this.userInfo = uni.getStorageSync('userInfo');
				this.addressList=uni.getStorageSync('addressList')
				// console.log(this.userInfo.id)
				// uni.request({
				// 	url: 'http://localhost:8002/address/pageQuery',
				// 	data: {
				// 		page: 1,
				// 		pageSize: 9,
				// 		userId: this.userInfo.id
				// 	},
				// 	success: (res) => {
				// 		console.log(res.data)
				// 		this.addressList = res.data.data.list;
				// 		uni.setStorageSync('addressList',this.addressList)
				// 		console.log(this.addressList)

				// 	}
				// })
			},
			// 新增地址
			addAddressbtn() {
				uni.navigateTo({
					url: '/pages/addAddress/addAddress'
				})
			},

			// 触发提交表单
			submitOrder() {
				this.prodatails = uni.getStorageSync("prodatails")[0]
				// console.log(this.addressId)
				uni.request({
					url: "http://localhost:8002/order/submitOrder",
					data: {
						addressId: this.addressId,
						customerId: this.userInfo.id,
						orderLines: [{
							// "id": 0,
							"number": this.number,
							// "orderId": 0,
							"price": this.prodatails.price * this.number,
							"productId": this.prodatails.id
						}],
						"serviceTime": this.single
					},
					method: 'POST',
					dataType: 'json',
					success: (res) => {
						if (res.data.status === 200) {
							uni.showToast({
								title: '预约成功！',
							},4000)
							// console.log(res)
							uni.redirectTo({
								url:'/pages/order/order'
							})
							
						}else{
							console.log(res)
							uni.showToast({
								title:res.data.message,
								icon:'error'
							},4000)
							
						}
					}
				})

			}
		}
	}
</script>

<style lang="scss">
	.forms {
		padding: 30rpx 40rpx;

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

		.example-body {
			background-color: #fff;
			padding: 10px;
		}

		.addcontent {
			margin-left: 60rpx;
			margin-top: -50rpx;
		}

		.btn {
			margin-top: 30rpx;
			width: 100%;
			height: 100rpx;
			background-color: #ffffff;
			color: #333;
			text-align: center;
			border-radius: 8rpx;
		}

	}

	.uni-container {
		height: 60rpx;

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
	}
</style>
