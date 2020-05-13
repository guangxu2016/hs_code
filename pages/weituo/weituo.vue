<template>
	<view class="weituo">
		<view @click="zhiDan">
			{{i18n.weituozhidan}}
		</view>
		<view @click="guanWu">
			{{i18n.yitihuafuwu}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		onLoad() {
			uni.setNavigationBarTitle({
				title: this.$t('index.weituos')
			})	
		},
		computed: {
			i18n() {
				return this.$t('index');
			}
		},
		methods: {
			zhiDan(){
				uni.switchTab({
				    url: '../cross/cross',
				});
			},
			guanWu(){
				uni.switchTab({
				    url: '../lock/lock',
				});
			}
		}
	}
</script>

<style>
.weituo view {
	width: 80%;
	height: 100rpx;
	line-height: 100rpx;
	background-color: #007AFF;
	margin: 0 auto;
	color: #fff;
	font-size: 32rpx;
	margin-top: 60rpx;
	text-align: center;
	border-radius: 40rpx;
}


</style>
