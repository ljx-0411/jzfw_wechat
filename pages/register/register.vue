<template>
	<view class="form">
		<view class="register">
			<!-- <u-avatar v-show="false" :src="imgsrc"></u-avatar> -->
			<uni-forms label-position="left" :label-style="{'font-size':'40rpx'}" :error-type="errorType" ref="uForm"
				:model="form">
				<uni-forms-item label="用户名:" required prop="username">
					<input class="input" type="text" v-model="form.username" placeholder="请输入用户名" />
				</uni-forms-item>

				<uni-forms-item label="密码:" required prop="password">
					<input class="input" type="password" v-model="form.password" password="true" placeholder="请输入密码" />
				</uni-forms-item>
				<uni-forms-item label="电话号码:" prop="telephone" maxlength="11">
					<input class="input" type="number" v-model="form.telephone" placeholder="请输入电话号码" />
				</uni-forms-item>
				<uni-forms-item label="性别：" labelWidth="150rpx">
					<uni-data-select v-model="value" :localdata="range" @change="change"></uni-data-select>
				</uni-forms-item>
				<uni-forms-item label="邮箱:" prop="email">
					<input class="input" type="email" v-model="form.email" placeholder="请输入邮箱" />
				</uni-forms-item>

				<button class="button" @tap="register">注册</button>
			</uni-forms>
			<view class="other">
				<navigator url="/pages/login/login">已有账户?点击登入</navigator>
			</view>
		</view>


	</view>

</template>

<script>
	export default {
		data() {
			return {
				errorType: ['message'],
				imgsrc: '',
				form: {
					username: '',
					password: '',
					telephone: '',
					email: '',
					gender: '',

				},
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
				//这里写的是一些验证规则
				rules: {
					username: {
						rules: [{
							required: true,
							errorMessage: '请输入用户名',
							trigger: ['change', 'blur']
						}, 
						// {
						// 	validateFunction: function(rule, value, data, callback) {
						// 		if (value.length < 6 || value.length > 8) {
						// 			callback('用户名不超过6')
						// 		}
						// 		return true
						// 	}
						// },
						]
					},

					password:{
						rules:[{
								pattern: /^[a-zA-Z]\w{5,17}$/, //正则表达式
								transform(value) {
									return String(value);
								},
								errorMessage: '以字母开头，长度在6~18之间，只能包含字母、数字和下划线',
								trigger: ['change', 'blur']
							},
							 {
								required: true,
								message: '请输入密码',
								trigger: ['change', 'blur']
							},
						
						],
					} ,
					telephone: [{
							required: true,
							message: '请输入手机号码',
							trigger: ['change', 'blur']
						},
						{
							validator: (rule, value, callback) => {
								return this.$u.test.telephone(value);
							},
							message: '手机号码不正确',
							trigger: ['change', 'blur']
						}, {
							pattern: /^(13[0-9]|14[5|7]|15[0|1|2|3|4|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$/,
							transform(value) {
								return String(value);
							},
							message: '手机号码输入不正确',
							trigger: ['change', 'blur']
						}
					],
					email: [{
						required: true,
						message: '请输入邮箱',
						trigger: ['change', 'blur']
					}, {
						pattern: /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/,
						message: '邮箱格式不正确',
						trigger: ['change', 'blur']
					}],

				},
				//这个是设置radio
				list: [{
						value: '男',
						disabled: false,
						gender: 0
					},
					{
						value: '女',
						disabled: false,
						gender: 1
					}
				],
				radio: '',
				img: ''
			}
		},
		mounted() {
			// this.register();
				this.$refs.uForm.setRules(this.rules);
		},
		onReady() {
			this.$refs.uForm.setRules(this.rules);
		},
		methods: {
			register() {
				this.$refs.uForm.validate(valid => {
					if (valid) {
						console.log('验证通过');
					} else {
						console.log('验证失败', valid);
						uni.showToast({
							title: '注册失败！',
							icon: 'error',
							mask: true,
							duration: 1500
						})
					}
				}).catch(err => {
					console.log('表单错误信息：', err);
				})
				uni.showLoading({
					title: '加载中..'
				})
				if (this.value == 0) {
					this.form.gender= 'male'
				} else {
					this.form.gender= 'female'
				}
				uni.request({
					url: 'http://localhost:8002/user/register', //写接口的
					data: {
						username: this.form.username,
						password: this.form.password,
						telephone: this.form.telephone,
						gender: this.form.gender,
						email: this.form.email,
					},
					method: "POST",
					success: res => {
						if (res.data.status == 200) { //注册成功就执行下面这些代码
							uni.hideLoading();
							// console.log(this.imgsrc);
							//this.login(res.data);
							uni.showToast({ //显示注册成功
								title: '注册成功',
								mask: true,
								duration: 1500
							})
							setTimeout(() => {
								uni.navigateTo({ //注册成功跳转登录页面
									url: '/pages/login/login',
								}, 3000);
							})
						} else {
							uni.showToast({
								title: res.data.msg,
								icon: "none"
							})
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
	.form {
		display: flex;
		height: 100vh;
		justify-content: center;

		.gender {
			margin-left: 10rpx;
		}

		.register {
			width: 100%;
			padding: 50rpx 50rpx;

			.uni-forms {

				// width: 600rpx;
				.input {
					width: 500rpx;
					height: 40rpx;
					margin-top: 20rpx;
					border: 1rpx solid #dadbde;
					padding: 12rpx 18rpx;
					border-radius: 8rpx;
					font-size: 28rpx;
					color: rgb(48, 49, 51);
				}

				.button {

					margin-top: 100rpx;

					width: 100%;
					height: 100rpx;
					background-color: #3c9cff;
					color: #fff;
					text-align: center;
					border-radius: 8rpx;
				}
			}
		}
	}

	.other {
		margin-top: 2vh;
		display: flex;
		justify-content: center;
		font-size: 30rpx;
		color: #333;
	}
</style>
