<template>
	<view>
		 <!-- 商品描述 用户选择hscode -->
		<view v-show="userShow" class="ueser_show">
			<view>{{ i18n.chooseChaXun }}</view>
			<view :class="{ checked: isCheck == index }" @click="userChooseClick(ins, index)" v-for="(ins, index) in userChoose" :key="index">
				<text>{{ ins }}</text>
			</view>
			<view>
				<text @click="chaxun">{{ i18n.search }}</text>
			</view>
		</view>
		<!-- true显示数据  默认为true -->

		<view v-show="flag">
			<view class="yeah_search" >
				<!-- 上面国家  已知 -->
			<view class="changeHead" v-show="aaaa">
				<text class="red">{{city_ones}}</text>
				<image @click="countryChange" :src="imgsjt" mode=""></image>
				<text>{{city_twes}}</text>
			</view>
			<!-- 数据内容  -->
			<view v-if="types == 'zhuanyi'">
				<view class="changeHead">
					<text class="red">{{city_ones}}</text>
					<image @click="countryChangeZhuanyi" :src="imgsjt" mode=""></image>
					<text>{{city_twes}}</text>
				</view>
					<view class="header_search" v-for="(ite,index) in attrList" :key="index" v-if="ite.value != ''">
							<view  v-if="system_infoLangu == 'zh-CN' || system_infoLangu == 'zh_CN'">{{ ite.cn_name }}</view>
							<view v-else>{{ ite.en_name }}</view>
							<view >{{ ite.value}}</view>
					</view>
					
				
					<!-- <view class="bbbb" v-show="cccc">
						<text class="red">{{city_twes}}</text>
					</view>
					<view class="header_search" v-for="(ite,index) in attrLists" :key="index" v-if="ite.value != ''">
							<view v-if="system_infoLangu == 'zh-CN' || system_infoLangu == 'zh_CN'">{{ ite.cn_name }}</view>
							<view v-else>{{ ite.en_name }}</view>
							<view >{{ ite.value}}</view>
					</view> -->
			</view>
			<!-- 描述 -->
			<view v-else-if="types == 'miaoshu'">
				<!-- <view class="bbbb" v-show="cccc">
					<text class="red">{{city_ones}}</text>
				</view> -->
				<view class="changeHead">
					<text class="red">{{city_ones}}</text>
					<image @click="countryChangeMiaoshu" :src="imgsjt" mode=""></image>
					<text>{{city_twes}}</text>
				</view>
					<view  v-for="(ite,index) in attrList" :key="index">
						<view class="header_search" v-for="ir in ite.data" v-if="ir.value != ''">
							<view v-if="system_infoLangu == 'zh-CN' || system_infoLangu == 'zh_CN'">{{ ir.cn_name }}</view>
							<view v-else>{{ ir.en_name }}</view>
							<view >{{ ir.value}}</view>
						</view>
							
					</view>

					<!-- <view class="bbbb" v-show="cccc">
						<text class="red">{{city_twes}}</text>
					</view> -->
					<!-- <view class="header_search" v-for="(ite,index) in attrLists" :key="index" >
						<view v-for="irs in ite.data" v-if="irs.value != ''">
							<view v-if="system_infoLangu == 'zh-CN' || system_infoLangu == 'zh_CN'">{{ irs.cn_name }}</view>
							<view v-else>{{ irs.en_name }}</view>
							<view >{{ irs.value}}</view>
						</view>
							
					</view> -->
			</view>
			<!-- 已知 -->
			<view v-else>
				<view class="header_search" v-for="(ite,index) in attrList" :key="index" v-if="ite.value != ''">
						<view v-if="system_infoLangu == 'zh-CN' || system_infoLangu == 'zh_CN'">{{ ite.cn_name }}</view>
						<view v-else>{{ ite.en_name }}</view>
						<view >{{ ite.value}}</view>
				</view>
			</view>
			</view>
		</view>

		<view class="not_search" v-show="hide">
			<!-- <image src="../../static/image/fdj.png" mode=""></image> -->
			<image src="../../static/image/search.png" mode=""></image>
			<view>{{ notSearch }}</view>
			<view @click="back">{{ i18n.back }}</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			flag: false,
			hide:false,
			city_one_name: '',
			city_one_index: '',
			city_two_name: '',
			city_two_index: '',
			search: '',
			num: '',
			local_langu: '',
			inter_langu: '',
			exit: '',
			generally: '',
			conCom: [],
			userShow: false,
			userChoose: [],
			isCheck: -1,
			result: '',
			code:"",
			en:"",
			zh:"",
			title:"",
			specliy:"",
			bianma:"",
			zhongwen:"",
			yingwen:"",
			shuilv:"",
			specshuilv:"",
			nums:0,
			types:"",
			attrList:[],
			system_infoLangu:"",
			status:1,
			imgsjt:"../../static/image/change.png",
			city_twes:"",
			city_ones:"",
			edqa:"",
			join:"",
			notSearch:"未搜索到HScode的结果~",
			timer:"",
			attrLists:[],
			aaaa:false,
			cccc:false,
			userChooses	:"",
			joins:"",
			sums:"",
			shuzu:[],
			shuzus:[]
		};
	},
	onLoad: function(option) {
		uni.setNavigationBarTitle({
				title: this.$t('index.chaxunjieguo')
		})	
		console.log(option.city_one_name);
		console.log(option.city_one_index);
		console.log(option.city_two_name);
		console.log(option.city_two_index);
		console.log(option.search);
		console.log(option.two_search);

		this.city_one_name = option.city_one_name;
		this.city_one_index = option.city_one_index;
		this.city_two_name = option.city_two_name;
		this.city_two_index = option.city_two_index;
		this.search = option.search;
		this.two_search = option.two_search;
		this.types = option.type
		let type = option.type;
		const system_info = uni.getStorageSync('system_info');
		console.log(system_info.language);
		this.system_infoLangu = system_info.language;
		// uni.showToast({
		//     title: this.system_infoLangu,
		// 	icon:"none",
		// 	duration: 2000
		// });
		console.log('type类型是' + type);
		var that = this;
		// 已知hscode查询
		if (type == 'KnowHScode') {
			// if(this.status == 1){
				that.aaa();
			// }
			
		} else if (type == 'zhuanyi') {
			// 转译接口
			var that = this;
			that.userShow = false;
			uni.request({
				url: 'http://td.gfapki.com.cn:8090/machine-interface-cch',
				method: 'get',
				data: {
					countryIndex1: that.city_one_index,
					countryIndex2: that.city_two_index,
					knownHsCodeForCountry1: that.search,
					maxNumberOfResults: '10'
				},
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				success: res => {
					console.log(res);
					console.log(res.data);
	
						if (res.data == '' || res.data == null ||res.data == undefined) {
							that.userShow = false;
							that.flag = false;
							that.hide = true;
						} else {
							that.userShow = true;
							that.userChoose = res.data;
							that.join = that.userChoose.join();
							that.miaoshu();
						}
				},
				fail: () => {},
				complete: () => {}
			});
		} else {
			var that = this;
			// 查询商品描述接口
// http://td.gfapki.com.cn:8090/machine-interface-cd?countryIndex=I & description=D&maxNumberOfResults=M
			uni.request({
				// url: 'http://td.gfapki.com.cn:8090/machine-interface-ccd',
				url:"http://td.gfapki.com.cn:8090/machine-interface-cd",
				method: 'get',
				data: {
					countryIndex: that.city_one_index,
					description: that.search,
					maxNumberOfResults: '10'
				},
				header: {
					'content-type': 'application/x-www-form-urlencoded'
				},
				success: res => {
					console.log(res.data);
					if(res.data == null || res.data == undefined || res.data == ""){
						that.flag = false;
						that.hide = true;
						return false;
					}
					for (var i = 0; i < res.data.length; i++) {
						if(res.data[i] == "" || res.data[i] == null){
							that.flag = false;
							that.hide = true;
							return false;
						}
					}
					// that.userShow = true;
					that.userChoose = res.data;
					that.join = that.userChoose.join();
					
					// that.miaoshu();
					uni.request({
						// url: 'http://td.gfapki.com.cn:8090/machine-interface-ccd',
						url:"http://td.gfapki.com.cn:8090/machine-interface-cd",
						method: 'get',
						data: {
							countryIndex: that.city_two_index,
							description: that.search,
							maxNumberOfResults: '10'
						},
						header: {
							'content-type': 'application/x-www-form-urlencoded'
						},
						success: res => {
							console.log(res.data);
							if(res.data == null || res.data == undefined || res.data == ""){
								that.flag = false;
								that.hide = true;
								return false;
							}
							for (var i = 0; i < res.data.length; i++) {
								if(res.data[i] == "" || res.data[i] == null){
									that.flag = false;
									that.hide = true;
									return false;
								}
							}
					
								that.userShow = true;
								that.userChooses = res.data;
								that.joins = that.userChooses.join();
								that.sums = that.join + that.joins;
								console.log(that.join);
								console.log(that.joins);
								// console.log(that.join + that.joins);
								console.log(that.sums);
								
								that.miaoshu();
							
						},
						fail: () => {},
						complete: () => {}
					});
			
						// that.userShow = true;
						// that.userChoose = res.data;
						// that.join = that.userChoose.join();
						// that.miaoshu();
					
				},
				fail: () => {},
				complete: () => {}
			});
		}
	},
	computed: {
		i18n() {
			return this.$t('index');
		}
	},
	methods: {
		 aaa(){
			 var that = this;
			that.userShow = false;
			that.baseUrl = that.websiteUrl;
			let token = uni.getStorageSync('storage_token');
			uni.request({
				url: that.baseUrl + '/api/search',
				method: 'GET',
				data: {
					hscode1: that.search,
					country_index1: that.city_one_index,
					country_name1: that.city_one_name,
					country_index2: that.city_two_index,
					country_name2: that.city_two_name,
					type: '1',
					hscode2: that.two_search
				},
				header: {
					'content-type': 'application/x-www-form-urlencoded',
					token: token
				},
				success: res => {
					console.log(res.data);
					// console.log(res.data.data[0]);
					if(res.data.code == "200") {
					if(res.data.data[0] == null || res.data.data[0] == undefined || res.data.data[0] == "") {
						that.flag = false;
						that.hide = true;
						return false;
					}else {
						that.flag = true;
						that.hide = false;
						that.aaaa= true;
						that.cccc = false;
					}
						console.log(res.data.data[0].data)
						
						if(that.status == 1){
							that.attrList = res.data.data[0].data;
							that.city_ones = that.city_one_name;
							that.city_twes = that.city_two_name;
							// that.city_twes = that.city_one_name;
						}else if(that.status == 2){
							that.attrList = res.data.data[1].data;
							that.city_ones = that.city_two_name;
							that.city_twes = that.city_one_name;
						}else {
							if(res.data.data[1] == null || res.data.data[1] == undefined || res.data.data[1] == "") {
								uni.showToast({
								    title: "请返回页面，查看是否填写正确",
									icon:"none",
									duration: 2000
								});
								return false;
							}
							that.attrList = res.data.data[1].data;
							
							
						}
						
					}else if(res.data.code == "202") {
						that.flag = false;
						that.hide = true;
						uni.showToast({
						    title: res.data.msg,
							icon:"none",
							duration: 2500
						});
					}
				},
				fail: () => {},
				complete: () => {}
			});
		},
		back() {
			uni.navigateBack();
		},
		countryChange(){
			if(this.types == "KnowHScode" ){
				if(this.status == 1) {
					this.status = 2;
					// 切换后数据
					console.log("2是切换了第一下"+this.status);
					this.aaa();
					this.city_ones = this.city_two_name;
					this.city_twes = this.city_one_name;
					
				}else {
					// 原来的
					this.status = 1;
					this.aaa();
					this.city_ones = this.city_one_name;
					this.city_twes = this.city_two_name;
					
				}
			}
		},
		countryChangeZhuanyi(){
			
			var arr= this.shuzu;
			var arr1 = this.shuzus;
			
			var s = arr;
			var ss = arr1;
			
			console.log(arr);
			console.log(arr1);
		
			if(this.types == "zhuanyi" ){
				if(this.status == '1') {
					this.status = 2;
					// 切换后数据
					console.log("2是切换了第一下"+this.status);
					this.city_ones = this.city_two_name;
					this.city_twes = this.city_one_name;
					this.attrList = ss;
					
				}else {
					// 原来的
					this.status = 1;
					console.log("1是切换了第一下"+this.status);
					this.city_ones = this.city_one_name;
					this.city_twes = this.city_two_name;
					this.attrList = s;
					
				}
			}
		},
		countryChangeMiaoshu(){
			var arr= this.shuzu;
			var arr1 = this.shuzus;
			
			var s = arr;
			var ss = arr1;
			
			console.log(arr);
			console.log(arr1);
					
			if(this.types == "miaoshu" ){
				if(this.status == '1') {
					this.status = 2;
					// 切换后数据
					console.log("2是切换了第一下"+this.status);
					this.city_ones = this.city_two_name;
					this.city_twes = this.city_one_name;
					this.attrList = ss;
					
				}else {
					// 原来的
					this.status = 1;
					console.log("1是切换了第一下"+this.status);
					this.city_ones = this.city_one_name;
					this.city_twes = this.city_two_name;
					this.attrList = s;
					
				}
			}
		},
		userChooseClick(e, index) {
			console.log(e);
			this.isCheck = index;
			this.result = e;
			this.nums = 1;
		},
		miaoshu(){
			console.log(this.result);
			var that = this;
			if(that.types == "miaoshu") {
				that.userShow = false;
				that.baseUrl = that.websiteUrl;
				const system_info = uni.getStorageSync('system_info');
				let token = uni.getStorageSync('storage_token');
				uni.request({
					url: that.baseUrl + '/api/search',
					method: 'GET',
					data: {
						hscode1: that.join,
						country_index1: that.city_one_index,
						country_name1: that.city_one_name,
						country_index2: that.city_two_index,
						country_name2: that.city_two_name,
						type: '2',
						hscode2:that.joins,
						content: that.search
					},
					header: {
						'content-type': 'application/x-www-form-urlencoded',
						token: token
					},
					success: res => {
						console.log(res);
						console.log(res.data);
						console.log(res.data.code);
						
					if(res.data.code == "200") {
						console.log("111111");
						if(res.data.data == null || res.data.data == undefined || res.data.data == "") {
							that.flag = false;
							that.hide = true;
							return false;
						}else {
							that.flag = true;
							that.hide = false;
						}
							that.attrList = res.data.data.data;
							that.city_twes = that.city_two_name;
							that.city_ones = that.city_one_name;
							
							console.log(that.attrList);
							
						}else if(res.data.code == "201"){
								that.flag = false;
								that.hide = true;
								that.notSearch = res.data.msg;
								
								console.log(res.data.data.id);
								
								that.timer = setInterval(function(){
							  		that.detail(res.data.data.id);
								},3000)
								
							}else if(res.data.code == "202") {
							that.flag = false;
							that.hide = true;
							uni.showToast({
							    title: res.data.msg,
								icon:"none",
								duration: 2500
							});
						}
					},
					fail: () => {},
					complete: () => {}
				});
				}else if(that.types == "zhuanyi"){
					// console.log(that.result);
					that.userShow = false;
					that.baseUrl = that.websiteUrl;
					const system_info = uni.getStorageSync('system_info');
					let token = uni.getStorageSync('storage_token');
					uni.request({
						url: that.baseUrl + '/api/search',
						method: 'GET',
						data: {
							hscode1: that.search,
							country_index1: that.city_one_index,
							country_name1: that.city_one_name,
							country_index2: that.city_two_index,
							country_name2: that.city_two_name,
							type: '3',
							hscode2: that.join
							// hscode2: this.result
						},
						header: {
							'content-type': 'application/x-www-form-urlencoded',
							token: token
						},
						success: res => {
							console.log(res);
							console.log(res.data);
							if(res.data.data == null || res.data.data == "" || res.data.data == undefined) {
								that.hide = true;
								that.flag = false;
							}
							
						if(res.data.code == "200") {
							if(res.data.data == null || res.data.data == undefined || res.data.data == "") {
								that.flag = false;
								that.hide = true;
								return false;
							}else {
								that.flag = true;
								that.hide = false;
							}
								
								that.attrList = res.data.data.data;
								// console.log(that.city_two_name);
								that.city_twes = that.city_two_name;
								that.city_ones = that.city_one_name;
								console.log(that.attrList);
	
							}else if(res.data.code == "201"){
								that.flag = false;
								that.hide = true;
								that.notSearch = res.data.msg;
								
								console.log(res.data.data.id);
								
								that.timer = setInterval(function(){
							  		that.detail(res.data.data.id);
								},3000)
								
							}else if(res.data.code == "202") {
								that.flag = false;
								that.hide = true;
								uni.showToast({
								    title: res.data.msg,
									icon:"none",
									duration: 2500
								});
							}
						},
						fail: () => {},
						complete: () => {}
					});
				}
		},
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
					// clearInterval(that.timer);
					if(res.data.code == 200){
						clearInterval(that.timer);
						that.flag = true;
						that.hide = false;
						that.city_twes = that.city_two_name;
						that.city_ones = that.city_one_name;
						that.aaaa = false;
						that.cccc = true;
						console.log(res.data.data)
						console.log(res.data.data.start);
						console.log(res.data.data.start.data);
						
						if(that.types == "zhuanyi"){
							that.attrList = res.data.data.start.data;
							that.attrLists = res.data.data.end.data;
							that.shuzu = res.data.data.start.data;
							that.shuzus = res.data.data.end.data;
						}else if(that.types == "miaoshu"){
							that.attrList = res.data.data.start;
							that.attrLists = res.data.data.end;
							that.shuzu = res.data.data.start;
							that.shuzus = res.data.data.end;
						}
					// 	console.log(that.attrList);
					// 	console.log(res.data.data.data.start)
						
					}
				}
			});
		},
		chaxun() {
			if(this.nums == "0") {
				uni.showToast({
				    title: "请选择",
					icon:"none",
					duration: 800
				});
				return false;
			}
		}
	}
};
</script>

<style>
page {
	/* background-color: #f1f1f1; */
}
.not_search {
	position: relative;
	top: 300rpx;
	text-align: center;
	/* background-color: #fff; */
}
.not_search image {
	width: 260rpx;
	height: 200rpx;
}
.not_search view:nth-child(2) {
	color: #bbb;
	font-size: 32rpx;
	margin-top: 20rpx;
}
.not_search view:last-child {
	width: 70%;
	height: 90rpx;
	line-height: 90rpx;
	text-align: center;
	border-radius: 40rpx;
	color: #fff;
	background-color: #007AFF;
	margin: 0 auto;
	margin-top: 50rpx;
}
.yeah_search {
	width: 92%;
	margin: 0 auto;
	/* border-top:1px solid #ccc; */
	padding: 40rpx 30rpx;
	padding-bottom: 50rpx;
	background-color: #fff;
	-moz-box-shadow: 0px 0px 2px 3px #dfdfdf;
	-webkit-box-shadow: 0px 0px 2px 3px #dfdfdf;
	box-shadow: 0px 0px 2px 3px #dfdfdf;
	border-radius: 20rpx;
	margin-top: 30rpx;
	margin-bottom: 60rpx;
	overflow: hidden;
	box-sizing: border-box;
}
.yeah_search .header_search {
	border-bottom: 1px solid #ccc;
	padding-bottom: 10rpx;
	box-sizing: border-box;
}
.header_search + .header_search {
	margin-top: 20rpx;
}
.yeah_search .header_search view {
	font-size: 28rpx;
	line-height: 40rpx;
	
}
.yeah_search .header_search >view {
	/* border-bottom: 1px solid #ccc; */
	/* padding-bottom: 10rpx; */
}
.yeah_search .header_search view:first-child {
	/* color: #111; */
	font-weight: bold;
}
.yeah_search .header_search view:last-child {
	margin-top: 10rpx;
}
.ueser_show {
	width: 80%;
	margin: 0 auto;
	padding: 30rpx 20rpx;
	border-radius: 20rpx;
	margin-top: 40rpx;
	-moz-box-shadow: 0px 0px 2px 3px #ccc;
	-webkit-box-shadow: 0px 0px 2px 3px #ccc;
	box-shadow: 0px 0px 2px 3px #ccc;
	overflow: hidden;
	box-sizing: border-box;
}
.ueser_show view {
	font-size: 36rpx;
	line-height: 50rpx;
}
.ueser_show view text {
	display: block;
}

.ueser_show view:last-child text {
	width: 200rpx;
	height: 60rpx;
	line-height: 60rpx;
	background-color: #007aff;
	color: #fff;
	text-align: center;
	border-radius: 20rpx;
	font-size: 34rpx;
	margin: 0 auto;
}
.checked {
	color: red;
	/* background: red; */
}

.changeHead {
	width: 100%;
	text-align: center;
	margin: 0 auto;
}
.changeHead text {
	font-size: 32rpx;
}

.changeHead image {
	width: 60rpx;
	height: 50rpx;
	margin: 0rpx 20rpx;
	vertical-align: middle;
}
.bbbb {
	margin-top: 20rpx;
	margin-bottom: 10rpx;
}
.red {
	color: red;
}
</style>
