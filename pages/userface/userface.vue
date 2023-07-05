<template>
	<view>
		<!-- <updateUser></updateUser> -->
		<uni-section title="上传头像" type="line">
					<view class="example-body">
						<uni-file-picker 
							v-model="imageValue" 
							:imageStyles="imageStyles"
							fileMediatype="image" 
							mode="grid"
							 @select="select" 
							 	@progress="progress" 
							@success="success" 
							@fail="fail" 
						/>
					</view>
					<button class="btn" @click="uploadFace">确认</button>
				</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imageValue:[],
				tempFilePaths:'',
				userInfo:''
			}
		},
		onLoad() {
			this.userInfo=uni.getStorageSync('userInfo');
		},
		methods: {
			// 修改上传头像
			uploadFace(){
				// console.log(this.tempFilePaths)
				// console.log(this.userInfo.id)
				uni.request({
					url:'http://localhost:8002/baseUser/alterUserface1',
					data:{
						id:this.userInfo.id,
						userface:this.tempFilePaths
					},
					method:'POST',
					success: (res) => {
						// console.log(res)
						if(res.data.status==200){
							uni.showToast({
								title: '修改成功！',
								icon:'success'
							});
						}
					}
				})
			},
			// 获取上传状态
						select(e){
										this.tempFilePaths = e.tempFilePaths[0];//e是获取的图片源	
							console.log('选择文件：',e)
						},
						// // 获取上传进度
						// progress(e){
						// 	console.log('上传进度：',e)
						// },
						// // 上传成功
						// success(e){
						// 	console.log('上传成功')
						// },
						
						// // 上传失败
						// fail(e){
						// 	console.log('上传失败：',e)
						// }
		}
	}
</script>

<style>
.example-body {
		padding: 10px;
		padding-top: 0;
	}
	.btn{
		margin-top: 40rpx;
		background-color:#FD5554;
	}
</style>
