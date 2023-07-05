<template>
	 <view class="menu-wrap">
		 <scroll-view scroll-y="true" >
			 <view class="proAll" @click="getProducts('')">
				 全部商品 	
			 </view>
		 	<view class="scroll-view-item" v-for="item in categoryList" :key="item.id" @click="getProducts(item.id)">
				{{item.name}}
			</view>
		 </scroll-view>
		 <view class="content" >
			 <image src="/static/img/banner.jpg" mode="widthFix"></image>
		
		 	<view class="row">
		 		<view class="products"  v-for="item in proList" :key="item.id" @click="getProDetails(item.name)">
		 			<view class="propic">
						<image :src="item.photo" mode=""></image>
		 			</view>
					<text class="proName">{{item.name}}</text>
		 		</view>
				<view class="noPro" v-if="!proList.length">
					<image src="/static/img/18.png" mode="aspectFill" ></image>
					<text>暂时没有内容哦~~~</text>
				</view>
		 	</view>
		 </view>
	        
	    </view>
</template>

<script>
	
	export default {
		data() {
			return {	
				categoryList:[],
				proList:[],
				
			}
		},
		onReady() {
		},
		onLoad() {
			this.getCategoryService();
			this.getProducts('');
		},
			
			methods: {
				// 从存储当中拿到服务分类数据
				getCategoryService(){
					this.categoryList=uni.getStorageSync('categoryList')
					// console.log(this.categoryList)
				},
				getProducts(id){
					uni.request({
						url:"http://localhost:8002/product/pageQuery",
						data:{
							page:1,
							pageSize:9,
							productCategoryId:id
						},
						success:(res)=>{
							// console.log(res.data.data.list);
							// console.log(id);
							this.proList=res.data.data.list
						}
					})
					
					
				},
				getProDetails(name){
				
					uni.request({
						url:"http://localhost:8002/product/pageQuery",
						data:{
							page:1,
							pageSize:9,
							name:name
						},
						success:(res)=>{
							// console.log(res.data.data.list);
							uni.setStorageSync('prodatails',res.data.data.list)
							// console.log(id);
							// this.proList=res.data.data.list
						}
					}),
					uni.navigateTo({
						url:'/pages/productsDetails/productsDetails',
						// success: (res)=>{
							
						// 	console.log(res)
						// }
					})
				}

			}
	} 	
	
</script>

<style lang="scss">
	.proAll{
		margin: 20rpx;
		width:150rpx;
		height: 80rpx;
		line-height: 72rpx;
		text-align: center;
	}
.scroll-view-item {
	    margin: 20rpx;
		width:150rpx;
		height: 80rpx;
		line-height: 72rpx;
		text-align: center;
	}
	.menu-wrap{
		display: flex;
		.content{
			padding: 10rpx;
			width: 500rpx;
			
			image{
				margin:20rpx auto;
				width: 500rpx;
			}
			.row{
				display: flex;
				flex-wrap: wrap;
				align-items: center;
				.products{
					margin: 20rpx 10rpx;
					width: 230rpx;
					height: 230rpx;
					text-align: center;
					
					.propic{
						display: block;
						
						image{
							width: 180rpx;
							height: 180rpx;
						}
					}
					
					.proName{
						text-align: center;
						
					}
				}
				
				.noPro{
					width: 400rpx;
					height: 400rpx;
					margin: 0 auto;
						text-align: center;
					image{
						display:inline-block;
						width: 400rpx;
						height: 300rpx;
					}
					text{
						margin: 10rpx auto;
						font-size: 24rpx
					}
				}
			}
			
		}
	}
</style>
