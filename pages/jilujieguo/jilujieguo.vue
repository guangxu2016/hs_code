<template>
	<view>
		<view v-if="shows">
			<view class="detail" v-if="leixing == '1'">
				<view v-for="item in list">
					<view>
						<view>
							<text class="cityss" v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ item.country_en_name }}:</text>
							<text class="cityss" v-else>{{ item.country_en_name }}:</text>
						</view>
					</view>
					<view v-for="ir in item.data">
						<text v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ ir.cn_name }}:</text>
						<text v-else>{{ ir.en_name }}:</text>
						<text>{{ ir.value }}</text>
					</view>
				</view>
			</view>
			<view class="detail" v-else-if="leixing == '3'">
				<view class="red">{{ city }}</view>
				<view v-for="item in list">
					<text v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ item.cn_name }}:</text>
					<text v-else>{{ item.en_name }}:</text>
					<text>{{ item.value }}</text>
				</view>

				<view class="red">{{ citys }}</view>
				<view v-for="item in attrLists">
					<text v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ item.cn_name }}:</text>
					<text v-else>{{ item.en_name }}:</text>
					<text>{{ item.value }}</text>
				</view>
			</view>
			<view class="detail" v-else-if="leixing == '2'">
				<view v-for="item in list">
					<view>
						<text class="cityss" v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ item.country_cn_name }}:</text>
						<text class="cityss" v-else>{{ item.country_en_name }}:</text>
					</view>
					<view v-for="ir in item.data">
						<text v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ ir.cn_name }}:</text>
						<text v-else>{{ ir.en_name }}:</text>
						<text>{{ ir.value }}</text>
					</view>
				</view>
				<view v-for="item in listend">
					<view>
						<text class="cityss" v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ item.country_cn_name }}:</text>
						<text class="cityss" v-else>{{ item.country_en_name }}:</text>
					</view>
					<view v-for="ir in item.data">
						<text v-if="system_info == 'zh_CN' || system_info == 'zh-CN'">{{ ir.cn_name }}:</text>
						<text v-else>{{ ir.en_name }}:</text>
						<text>{{ ir.value }}</text>
					</view>
				</view>
			</view>
		</view>
		<view v-else class="zanwu">{{ seatchinfo }}</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			list: [],
			listend:[],
			attrLists: [],
			system_info: '',
			leixing: '',
			shows: true,
			city: '',
			citys: '',
			seatchinfo: '暂无数据，请核对下查询记录'
		};
	},

	onLoad(option) {
		console.log(option.id);
		console.log('类型' + option.leixing);
		this.leixing = option.leixing;
		this.detail(option.id);
		const system_info = uni.getStorageSync('system_info');
		console.log(system_info.language);
		this.system_info = system_info.language;

		uni.setNavigationBarTitle({
			title: this.$t('index.jilus')
		});
	},
	methods: {
		detail(id) {
			var that = this;
			that.baseUrl = that.websiteUrl;
			let token = uni.getStorageSync('storage_token');

			uni.request({
				url: that.baseUrl + '/api/logDetail',
				data: {
					id: id
				},
				method: 'GET',
				dataType: 'json',
				header: {
					'content-type': 'application/x-www-form-urlencoded',
					token: token
				},
				success: function(res) {
					console.log(res.data);
					if (res.data.data == '' || res.data.data == null || res.data.data == undefined) {
						that.shows = false;
						console.log('111');
					}
					if (res.data.code == 201) {
						that.shows = false;
						that.seatchinfo = res.data.msg;
						return false;
					}

					if (res.data.code == 200) {
						// 类型 1已知  3 转译  2 商品描述
						that.shows = true;
						console.log(that.leixing);
						if (that.leixing == 2) {
							that.list = res.data.data.start;
							that.listend = res.data.data.end;
							console.log(that.list);
							// that.attrLists = res.data.data.end.data;
						} else if (that.leixing == 1) {
							console.log('111');
							that.list = res.data.data;
						} else if (that.leixing == 3) {
							that.list = res.data.data.start.data;
							that.attrLists = res.data.data.end.data;
							that.city = res.data.data.start.country_en_name;
							that.citys = res.data.data.end.country_en_name;
						}

						console.log(that.list);

						if (that.list == '' || that.list == undefined || that.list == null) {
							that.shows = false;
						}

						// console.log(that.list.value);
					}
					// console.log(res.data.code);
				}
			});
		}
	}
};
</script>

<style>
.detail {
	width: 92%;
	margin: 0 auto;
	margin-top: 40rpx;
	background-color: #fff;
	padding: 20rpx;
	border-radius: 20rpx;
	-moz-box-shadow: 0px 0px 2px 3px #dfdfdf;
	-webkit-box-shadow: 0px 0px 2px 3px #dfdfdf;
	box-shadow: 0px 0px 2px 3px #dfdfdf;
	overflow: hidden;
	box-sizing: border-box;
}
.detail view > view {
	/* height: 60rpx; */
	line-height: 60rpx;
}
.detail text {
	font-size: 30rpx;
}
.detail view text:first-child {
	color: #555;
}
.detail view text:last-child {
	color: #333;
}
.detail view text:last-child {
	margin-left: 10rpx;
}

.detail view text:last-child {
	/* font-weight: 700; */
	color: #000000;
}

.zanwu {
	text-align: center;
	margin-top: 200rpx;
	color: #888;
	font-size: 32rpx;
}
.red,
.cityss {
	color: red !important;
	margin-top: 20rpx;
	margin-bottom: 10rpx;
}
.cityss {
	font-size: 36rpx !important;
	margin-bottom: 0rpx !important;
}
</style>
