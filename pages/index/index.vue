<!-- 家政服务首页 -->
<template>
	<view class="content">
		<view class="uni-margin-wrap">
			<view class="banner"></view>
			<swiper class="swiper" :indicator-dots="indicatorDots" indicator-active-color="#FD5554" :autoplay="autoplay"
				:interval="interval" :duration="duration">
				<swiper-item v-for="item in picList" :key="item.id">
					<view class="swiper-item uni-bg-red">
						<image :src="item.url" mode=""></image>
					</view>
				</swiper-item>
			</swiper>
		</view>
		<view class='menu-wrap'>
			<view class="menu" v-for="item in categoryList" :key="item.id" bindtap='setCurrentActive'>
				<view class="icon_img">
					<img :src="item.icon" alt="">
				</view>
				<text>{{item.name}}</text>
			</view>
		</view>
		<!-- 公告栏 -->

		<uni-notice-bar show-icon text="家政服务,带给你美好的生活服务体验!" />
		<uni-section title="家政资讯" type="line">
			<uni-card v-for="item in detailList" :key="item.id">
				<view class="">
					{{item.title}}
				</view>
				<view class="content1" v-html="item.content"></view>
				<navigator :url="`../inforDetail/inforDetail?id=${item.id}`">
					<text class="xiangqing" >详情</text>
				</navigator>
				<uni-icons type="star-filled" color="#999" size="18"></uni-icons>
			</uni-card>
		</uni-section>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 轮播图片数组
				picList: [],
				indicatorDots: true,
				autoplay: true,
				interval: 2000,
				duration: 500,
				// 服务分类
				categoryList: [],
				// 咨询分类
				inforList: [],
				userInfo: '',
				addressList: '',
				detailList: []
			}
		},
		onLoad() {
			// 数据加载中的模态框
			uni.showLoading({
				title: '数据加载中......',
				mask: false
			});
			this.getList();
			this.getCategory();
			this.getInformation();
			// 加载成功关闭模态框
			uni.hideLoading();
			uni.getStorageSync('userInfo')
			this.getAddress()

		},
		onShow() {
			this.getInformation()
		},
		methods: {
			goDetail(id) {
				uni.navigateTo({
					url: `../inforDetail/inforDetail?id=${id}`
				})
			},
			getAddress() {
				this.userInfo = uni.getStorageSync('userInfo');
				console.log(this.userInfo.id)
				uni.request({
					url: 'http://localhost:8002/address/pageQuery',
					data: {
						page: 1,
						pageSize: 9,
						userId: this.userInfo.id
					},
					success: (res) => {
						console.log(res.data)
						this.addressList = res.data.data.list;
						uni.setStorageSync('addressList', this.addressList)
						console.log(this.addressList)

					}
				})
			},
			// 获取轮播信息
			getList() {
				uni.request({
					url: 'http://localhost:8002/carousel/findAll',
					success: (res) => {
						this.picList = res.data.data;
						// console.log(res.data.data);

					}
				});
			},
			// 获取分类信息
			getCategory() {
				uni.request({
					url: 'http://localhost:8002/productCategory/pageQuery',
					data: {
						page: 1,
						pageSize: 99
					},
					success: (res) => {
						this.categoryList = res.data.data.list
						// console.log(res.data.data);
						// 将服务分类数据存储
						uni.setStorageSync('categoryList', this.categoryList);

					}
				});
			},
			// 获取资讯信息
			getInformation() {
				uni.request({
					url: 'http://localhost:8002/article/pageQuery',
					data: {
						page: 1,
						pageSize: 10,
					},
					success: (res) => {
						this.detailList = res.data.data.list
						console.log(res.data.data.list)
					}
				})
			},
			getInforDetail(id) {
				// 点击资讯列表跳转详情
				// console.log(id)
				uni.navigateTo({
					url: `../inforDetail/inforDetail?id=${id}`,

				});
			}

		}
	}
</script>

<style lang="scss">
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		color: #333;

		.uni-card {
			height: 150rpx;

			.content1 {
				width: 500rpx;
				padding-left: 10rpx;
				display: -webkit-box;
				-webkit-box-orient: vertical;
				-webkit-line-clamp: 1;
				white-space: nowrap;
				overflow: hidden;
				text-overflow: ellipsis;
			}

			.xiangqing {
				position: absolute;
				top: 95rpx;
				right: 60rpx;

			}

		}

		.uni-margin-wrap {
			width: 100%;
		}

		.banner {
			height: 350rpx;
			background-color: #FD5554;
			border-radius: 8px;
			box-shadow: 0 0 10px 0 #aaaaaa;
		}

		.swiper {
			height: 500rpx;
			width: 90%;
			position: absolute;
			top: 136px;
			left: 50%;
			transform: translate(-50%, -50%);
			border-radius: 8px;
		}

		.swiper-item {
			display: block;
			margin-top: 80rpx;
			height: 200rpx;
			text-align: center;

			image {
				border-radius: 50rpx;
				height: 350rpx;

			}
		}

		.menu-wrap {
			margin-top: 150rpx;
			display: flex;
			flex-wrap: wrap;
			// flex: 0 0 auto;
			align-items: center;
			justify-content: center;
			border-right: 1px solid #eee;
			padding: 10rpx 20rpx;

			.menu {
				margin: 10rpx 35rpx;

				.icon_img {
					width: 100rpx;
					height: 100rpx;
					display: flex;
					flex-wrap: wrap;
					align-items: center;
					justify-content: center;

					img {
						display: block;
						width: 80rpx;
						height: 80rpx;
					}
				}

				text {
					font-size: 24rpx;

				}
			}
		}

		.uni-section {

			width: 650rpx;

			.uni-list-item {
				height: 150rpx;
			}

			.uni-icons {
				position: absolute;
				top: 95rpx;

				right: 10rpx;
			}
		}

		// .information {
		// 	margin: 30rpx;
		// 	padding-left: 20rpx;
		// 	width: 660rpx;
		// 	display: flex;
		// 	flex-wrap: wrap;

		// 	.inforContent {
		// 		margin: 10rpx 15rpx;
		// 		padding: 0 5rpx;
		// 		width: 280rpx;
		// 		height: 200rpx;
		// 		background-color: #f5f5f5;
		// 		border-radius: 10rpx 10rpx 10rpx 10rpx;
		// 		box-shadow: 2px 2px 2px #808080;

		// 		.name {
		// 			display: block;
		// 			margin: 10px 5rpx;
		// 			font: 38rpx bold;
		// 		}

		// 		.desc {
		// 			padding: 20rpx;
		// 			font-size: 28rpx;
		// 		}
		// 	}
		// }
	}
</style>
