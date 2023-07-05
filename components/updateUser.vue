<template>
	<view>	
				<uni-file-picker 
					v-model="userUrl" 
					fileMediatype="image" 
					mode="grid" 
					@select="select" 
					@progress="progress" 
					@success="success" 
					@fail="fail" 
				/>
	
	</view>
</template>


				<script>
				export default {
					name:"updateUser",
				  data() {
				    return {
				      userUrl: ''
				    }
				  },
				  methods: {
				   select(e){
				   				console.log('选择文件：',e)
				   			},
				   			// 获取上传进度
				   			progress(e){
				   				console.log('上传进度：',e)
				   			},
				   			
				   			// 上传成功
				   			success(e){
				   				console.log('上传成功')
				   			},
				   			
				   			// 上传失败
				   			fail(e){
				   				console.log('上传失败：',e)
				   			}
				}
				</script>


<style>

</style>