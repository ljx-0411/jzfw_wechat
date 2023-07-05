<template>
<!-- //@touchmove.prevent不让遮罩层后的页面滚动 -->
<!-- //@tap.self只触发自己，或者给mask的子级.content加@tap.stop阻止事件默认行为 -->
	<view @touchmove.prevent :class="sc" class="tankuang width-100 height-100 position-fixed p-left-0 p-top-0">
		<view class="mask width-100 height-100"  @tap.self="$emit('maskTap')">
			<view class="content width-100 position-absolute p-left-0 bj-white p-30" >
				<slot></slot>
				<!-- 按钮 -->
				<!--  @tap="$emit('bottomTap')" class="bottomBtn width-100 bj-primary-co$emit('bottomTap')自定义事件 -->
			<view @tap="$emit('bottomTap')" class="bottomBtn width-100 bj-primary-color text-center text-white position-fixed p-bottom-0 p-left-0">{{title}}</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props:{
			title:String,//按钮内容
			sc:String //控制calss类名
		},
		data() {
			return {
			};
		},
		methods:{
			
		}
	}
</script>

<style>
	/* 底部弹框 */
	.mask {
		background-color: rgba(0, 0, 0, 0.5);
	}

	.content {
		height: 1037upx;
		border-radius: 20upx 20upx 0 0;
		box-sizing: border-box;
		bottom: 0;
	}

	.none {
		display: none;
	}

	@keyframes contentHide {
		from {
			transform: translateY(0%);
		}

		to {
			transform: translateY(100%);
		}
	}

	@keyframes maskHide {
		from {
			opacity: 1;
		}

		to {
			opacity: 0;
		}
	}

	.hide .mask {
		animation: maskHide 0.2s linear both;
	}

	.hide .mask .content {
		animation: contentHide 0.2s linear both;
	}

	@keyframes contentShow {
		from {
			transform: translateY(100%);
		}

		to {
			transform: translateY(0%);
		}
	}

	@keyframes maskShow {
		from {
			opacity: 0;
		}

		to {
			opacity: 1;
		}
	}

	.show {
		display: block;
	}

	.show .mask {
		animation: maskShow 0.2s linear both;
	}

	.show .mask .content {
		animation: contentShow 0.2s linear both;
	}
	
	/* 底部按钮 */
	.bottomBtn {
		height: 100upx;
		line-height: 100upx;
		font-size: 26upx;
	}
</style>

