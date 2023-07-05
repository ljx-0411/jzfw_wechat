<template>
	<view class="box">
		<view class="login">
			<view class="logins">
				家政服务登录
			</view>
			<view class="inputbox">
				<uni-forms label-position="left" :error-type="errorType" ref="userForm" :model="form">
					<uni-forms-item   prop="userName">
						<input class="input"  type="text" v-model="form.userName" placeholder="请输入用户名" />
					</uni-forms-item>
					<uni-forms-item  prop="password">
						<input  class="input" type="password" v-model="form.password" password="true" placeholder="请输入密码" />
					</uni-forms-item>
				</uni-forms>
			</view>
			<view>
				<button  @tap="login" class="button">登录</button>  
				<view class="other">
					<navigator url="/pages/register/register">注册账号</navigator>
					<text>|</text>
					<navigator url="/pages/forget/forget">忘记密码</navigator>
				</view>
			</view>
		</view>
		
	</view>
</template>
 
<script>
	export default {
		data() {
			return {
				token:'',
				errorType: ['message'],
				form: {
					userName: '',
					password: ''
				},
				hasLogin: false,
                //下面这个你可以看一下官网，他解释比较详细
				rules: {
					userName: [{
						required: true,
						message: '请输入用户名',
						trigger: 'change'
					}],
					password: [{
						required: true,
						message: '请输入密码',
						trigger: 'change'
					}]
				}
			}
		},
		onLoad() {
			
		},
		onReady() {    //这个跟上面那个rules结合一起用的
		// console.log(this.$refs)
			this.$refs.userForm.setRules(this.rules);
		},
		methods: {
			login() {
				const data = {
					userName: this.form.userName,
					password: this.form.password
				};
				this.$refs.userForm.validate(valid => {   //这个是如果用户名和密码没有填写就不进入下一项操作
						if (valid) {
							console.log('验证通过');
							
						} else if (this.form.password == '' || this.form.userName=='') {
							// console.log('请输入密码');
							uni.showToast({
								title:'请输用户名密码',
								icon:'error'
							})
							// console.log('验证失败');
						}
						
					}),
					
					uni.request({
						url:'http://localhost:8002/user/login',  //这里写的是登录接口
						data: {
							username: this.form.userName,
							password: this.form.password
						},
						method: 'POST',
						dataType:'json',
						success: (res) => {
							console.log(res);
							if (res.data.status == 200) {   //这里做了个判断，报200就是代表成功
								uni.hideLoading();
								uni.showToast({          
									title: '登录成功',
									mask: true,
									duration: 1500
								})
								this.hasLogin = true;
								uni.setStorageSync('token',res.data.data.token);   //登录成功保存token
								
								
								this.token=uni.getStorageSync('token'); 
								uni.request({
									url:'http://localhost:8002/user/info',
									data:{
										token:this.token
									},success:(res)=>{
										this.userInfo=res.data.data
										uni.setStorageSync('userInfo',this.userInfo)
										console.log(res.data.data)
										setTimeout(() => {
											//登录成功就跳转首页
											// console.log(this.userInfo.roles)
											if(this.userInfo.roles[0].id==1){
												uni.navigateTo({
													url: '/pages/empOrder/empOrder'
												}, 2000)
											}else if(this.userInfo.roles[0].id==6){
												uni.switchTab({
													url: '/pages/index/index'
												}, 2000)
											}
										})
									}
								})
								// console.log(res.data.data.token)
							} else if(res.data.status==500) {      
								console.log(res)
								uni.hideLoading();
								uni.showToast({       //如果没有登录成功，就显示那里错了
									title: res.data.message,
									icon: 'error'
								})
							}
						}
					})
					
				uni.showLoading({
					title:'登录中...',
					icon:'loading'
				})
			
			},
			
		}
	}
</script>
 
<style lang="scss" scoped>   
.box{
	height: 100vh;
	display: flex;
	justify-content: center;
	align-items: center;
	.login{
		.logins {
			text-align: center;
			font-size: 30px;
			color: #333333;
		}
		.inputbox{
			margin-top: 20rpx;
			width: 600rpx;
			.input{
				height: 60rpx;
				margin-top: 50rpx ;
					border: 1rpx solid #dadbde;
					padding: 12rpx 18rpx;
					border-radius: 8rpx;
					font-size: 30rpx;
					color: rgb(48, 49, 51);
						
			}
				.uni-forms-item{
					width: 200rpx;
					height: 100rpx;
					margin: 20rpx auto;
	
			}
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
	 
		.other {
			margin-top: 2vh;
			display: flex;
			justify-content: center;
			font-size: 30rpx;
			color: #333;
			::v-deep text {
				margin: 0 2vw;
			}
		}
	}
}

</style>