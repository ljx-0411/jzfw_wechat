<template>
	<view>
		<view class='container'>
		  <view class='user'>
		    <navigator url="../userface/userface">
		      <img class='userimg' :src='userInfo.userFace'/>
		    </navigator>
		    <view class='userinfo'>
		      <text class='username'>你好！{{userInfo.username}}</text>
		      <text class='tel'>电话：{{userInfo.telephone}}</text>
		    </view>
		    <navigator class='code' url='../user-code/user-code'>
		    <!--  <img src='/static/img/user/user-code.png'/> -->>
		    </navigator>
		  </view>
		  <view class='account'>
		    <navigator class='item' url=''>
		      <text class='text'>余额</text>
		      <text class='num'>{{userInfo.balance}}<text>元</text></text>
		    </navigator>
		    <navigator class='item' url='../coupons-my/coupons-my'>
		      <text class='text'>优惠券</text>
		      <text class='num'>3 <text>个</text> </text>
		    </navigator>
		  </view>
		 <view class='order'>
		
		  </view>
		  <view class='list'>
		    <view class='section'>
		      <navigator class='row' url='../userEdit/userEdit'>
		        <view class='left'>
		          <img class='icon' src='/static/img/user/wodedianpu.png'/>
		          <text class='text'>我的资料</text>
		        </view>
		        <img class='arrow' src='/static/img/arrow-right.png'/>
		      </navigator>
		   
		     
		      <navigator class='row'  url='../empOrder/empOrder'>
		        <view class='left'>
		          <img class='icon' src='/static/img/user/wodedingdan.png'/>
		          <text class='text'>我的订单</text>
		        </view>
		        <img class='arrow' src='/static/img/arrow-right.png'/>
		      </navigator>
			  <navigator class='row' url='../accountDetail/accountDetail'>
			    <view class='left'>
			      <img class='icon' src='/static/img/dingdan.png'/>
			      <text class='text'>账单明细</text>
			    </view>
			    <img class='arrow' src='/static/img/arrow-right.png'/>
			  </navigator>
		    </view>
		   
		   
			<button class="button" @click="logout">退出登录</button>
		  </view>
		</view>
		<view class="orderbtn" @click="goOrder">
			<img class='img'  src="/static/img//dingdan_1.png" alt="">
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				token:'',
				userInfo:{}
			}
		},
		onLoad() {
			this.getUserInfo()
		},
		onReady() {
		},
		onShow() {
			this.getMoney();
			this.getUserInfo()
		},
		methods: {
			goOrder(){
				uni.navigateTo({
					url:'/pages/empOrder/empOrder'
				})
			},
			// 充值成功后返回user更新余额
			getMoney(){
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
			},
			getUserInfo(){
				// console.log(uni.getStorageInfoSync("token"))
			this.userInfo=uni.getStorageSync('userInfo')
			// console.log(this.userInfo.balance)
			},
			async logout(){
				// 询问用户是否退出登录
				  const [err, succ] = await uni.showModal({
				    title: '提示',
				    content: '确认退出登录吗？'
				  }).catch(err => err)
				
				  if (succ && succ.confirm) {
				     // 用户确认了退出登录的操作
				    // 清空用户信息
					uni.setStorageSync('userInfo','')
					uni.setStorageSync('token','')
					uni.clearStorageSync()
					uni.reLaunch({
					url:'/pages/login/login',
					success:(res)=>{
						uni.showToast({
							title:'退出成功！'
						})
					}
				})
			}
		}
	},
	}
</script>

<style lang="scss">
.user {
  display: flex;
  position: relative;
  padding: 50rpx 40rpx 100rpx 40rpx;
  background-color: #FD5554;
  color: #fff;
  margin-bottom: 10rpx;
}
.user .code{

position:absolute;
top:100rpx;
right:40rpx;
}

.user .code img{
  top: 44rpx;
width:90rpx;
height:45rpx;

}

.userimg {
  width: 160rpx;
  height: 160rpx;
  border-radius: 50%;
  margin-right: 20rpx;
  background-color: #fff;
}

.userinfo {
  display: flex;
  flex-direction: column;
  line-height: 1;
  justify-content: space-around;
}

.username {
  font-size: 40rpx;
  /* line-height: 100rpx; */
}

.tel {
  font-size: 24rpx;
}

.cell {
  font-size: 24rpx;
  padding: 4rpx 12rpx;
  line-height: 1;
  border: 1px solid #fff;
  background-color: #FD5554;
  border-radius: 24rpx;
  margin-right: 30rpx;
}

.state {
  position: absolute;
  top: 60rpx;
  right: 40rpx;
  height: 60rpx;
  line-height: 60rpx;
  padding: 0 20rpx;
  border: 1px solid #fff;
  color: #fff;
  border-radius: 10rpx;
}

.list {
}

.list .section {
  padding-left: 40rpx;
  margin-bottom: 20rpx;
  background-color: #fff;
}

.row {
  position: relative;  
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #eee;
  padding: 30rpx 0;
}

.row .left {
  display: flex;
  align-items: center;
}

.row .icon {
  width: 40rpx;
  height: 35rpx;
  margin-right: 30rpx;
}

.row .arrow {
  width: 13rpx;
  height: 24rpx;
  margin-right: 30rpx;
  vertical-align: middle;
}

.tip {
  color: #d81e06;
  vertical-align: middle;
}

.account {
  display: flex;
  background-color: #fff;
  margin-bottom: 10rpx;
  width: 90%;
  margin: 0 auto;
  border-radius: 20rpx;
  position: absolute;
  top: 250rpx;
  left: 0;
  right: 0;
  box-shadow: 0px 0px 5px #ccc;
  color: #FD5554;
}

.account .item {
  display: flex;
  flex-direction: column;
  justify-content: center;
  flex: 0 0 50%;
  width: 50%;
  text-align: center;
  padding: 30rpx 0;
}

.account .text {
  /* font-size: 24rpx; */
  margin-bottom: 20rpx;
}

.account .num {
  font-size: 36rpx;
  color: #FD5554;
}

.account .num text {
  font-size: 24rpx;
}

.order {
  display: flex;
  margin-bottom: 20rpx;
  background-color: #fff;
  padding:100rpx 5% 0 5%;
box-sizing:border-box;

}

.order .item {
  display: flex;
  flex-direction: column;
  justify-content: center;
  flex: 0 0 50%;
  width: 50%;
  text-align: center;
  padding: 20rpx 0;
}

.order .icon {
  width: 50rpx;
  height: 50rpx;
  margin: 0 auto;
  margin-bottom: 20rpx;
}
.section button{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
}
.button{
	
	width: 100%;
	height: 100rpx;
		background-color: #FD5554;
		color: #fff;
		text-align: center;
		
		position: absolute;
		bottom: 10rpx;
}
.orderbtn{
		width: 100rpx;
		height: 100rpx;
		position: fixed;
		top: 1000rpx;
		right: 10rpx;
		// background-color: #FD5554;
		border-radius: 50rpx;
		color: #fff;
		text-align: center;
		line-height: 90rpx;
		.img{
			width: 80rpx;
			height: 80rpx;
		}
	}
</style>
