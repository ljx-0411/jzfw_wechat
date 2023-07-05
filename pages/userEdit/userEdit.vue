<template>
	<view>
		<view class="uni-form-item ">
			<uni-forms-item label="用户名:" labelWidth="150rpx">
				<input class="input" type="text" v-model="userInfo.username" placeholder="用户名" />
			</uni-forms-item>
			<uni-forms-item label="密码：" labelWidth="150rpx">
				<input class="input" type="text" v-model="userInfo.password" password="true" placeholder="密码" />
			</uni-forms-item>
			<uni-forms-item label="性别：" labelWidth="150rpx">
				<uni-data-select v-model="value" :localdata="range" @change="change"></uni-data-select>
			</uni-forms-item>
			<uni-forms-item label="手机号：" labelWidth="150rpx">
				<input class="input" type="text" v-model="userInfo.telephone" placeholder="手机号" />
			</uni-forms-item>
			<uni-forms-item label="注册时间：" labelWidth="200rpx">
				<view class="time">
					{{userInfo.registerTime|formatDate}}
				</view>
			</uni-forms-item>
			<button @click="updateInfo">确认修改</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				userInfo: '',
				value: 0,
				range: [{
						value: 0,
						text: "男"
					},
					{
						value: 1,
						text: "女"
					},
				],
			}
		},
		onLoad() {
			this.getInfo()
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
			change(e) {
				// 性别下拉框改变发生事件
				// console.log("e:", e);
			},
			getInfo() {
				this.userInfo = uni.getStorageSync('userInfo')
				if (this.userInfo.gender == 'male') {
					this.value = 0
				} else {
					this.value = 1
				}
			},
			updateInfo() {
				if (this.value == 0) {
					this.userInfo.gender = 'male'
				} else {
					this.userInfo.gender = 'female'
				}
				uni.request({
					url: 'http://localhost:8002/baseUser/saveOrUpdate',
					data: {
						id: this.userInfo.id,
						username: this.userInfo.username,
						password: this.userInfo.password,
						gender: this.userInfo.gender,
						telephone: this.userInfo.telephone
					},
					method: 'POST',
					success: (res) => {
						console.log(res.data)
						if (res.data.status == 200) {

							uni.showToast({
								title: '修改成功！！',
								icon: 'success',
								duration: 2000
							})
							setTimeout(() => {
								uni.navigateBack({
									delta: 1,

								})
							}, 2000)
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.input {
		height: 50rpx;
		width: 400rpx;
		margin-top: 10rpx;
		border: 1rpx solid #dadbde;
		padding: 12rpx 18rpx;
		border-radius: 8rpx;
		font-size: 30rpx;
		color: rgb(48, 49, 51);

	}

	.uni-forms-item {
		width: 200rpx;
		height: 100rpx;
		margin: 20rpx 100rpx;

		.time {
			width: 400rpx;
			line-height: 70rpx;
		}

		
	}
</style>
