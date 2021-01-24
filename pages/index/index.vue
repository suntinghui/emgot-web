<template>
	<view class="content">
		<image class="logo" src="/static/logo.png"></image>
		<view class="text-area">
			<text class="title">
				惠众宝 - 您身边的权益汇聚专家
			</text>
		</view>
		<view class="button-demo">
			<u-button type="primary" size="medium" :ripple="true" @click="yaoApp" >在App中打开</u-button>
		</view>
		<view class="company-info">
			<text class="info">
				易商惠众（北京）科技有限公司
			</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: "", // 商品id
				appExist: false,
				
			}
		},
		onLoad(option) {			
			// this.checkAppExist()
		},
		methods: {
			// 从请求地址中获得参数
			getQueryString(name) {
			   var reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)', 'i');
			   var r = window.location.search.substr(1).match(reg);
			   if (r != null) {
			        return unescape(r[2]);
			   }
			   return null;
			 },
			 
			 // 检查应用是否已安装
			 checkAppExist() {
				 //#ifdef APP-PLUS
				this.appExist = plus.runtime.isApplicationExist({pname:'com.cfastech.yonghu',action:'emgoth://'})
				if (this.appExist) {
					console.log("app已安装")
				} else {
					console.log("app没有安装")
				}
				//#endif
			 },
			 
			 openApp() {
				 console.log("打开应用")
				 
				 //#ifdef APP-PLUS
				 var that = this;
				 
				 var appurl = "emgoth://index"
				 plus.runtime.openURL(appurl, function(err) {
				 	console.log(err);
				 	uni.showModal({
				 		content:"本机未检测到惠众宝App，是否现在安装？",
				 		success:function(res){
				 			if (res.confirm) {
				 				that.installApp();
				 			}
				 		}
				 	})
				 });
				 
				 //#endif
			 },
			 
			 installApp() {
				 console.log("安装应用")
				 
				//#ifdef APP-PLUS
				var appurl;
				if (plus.os.name=="Android") {
					appurl = "market://details?id=com.cfastech.yonghu";
				} else {
					appurl = "itms-apps://itunes.apple.com/cn/app/%E6%83%A0%E4%BC%97%E5%AE%9D/id1541279288";
				}
				
				// http://www.html5plus.org/doc/zh_cn/runtime.html#plus.runtime.openURL
				plus.runtime.openURL(appurl, function(err) {
					console.log(err);
					uni.showToast({
						title: "打开失败"
					})
				});
				//#endif
			 },
			 
			 // https://www.jianshu.com/p/21380058d609
			 yaoApp() {
				var u = navigator.userAgent;
				var isWeixin = u.toLowerCase().indexOf('micromessenger') !== -1; // 微信内
				var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //android终端
				var isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端

				// 微信内
				if(isWeixin) {
					// uni.showModal({
					// 	title: '提示',
					// 	content: '请在浏览器上打开',
					// 	showCancel: false
					// })
					if (isIOS) {
						window.location.href = "itms-apps://itunes.apple.com/cn/app/%E6%83%A0%E4%BC%97%E5%AE%9D/id1541279288";
					} else if (isAndroid) {
						window.location.href ="https://a.app.qq.com/o/simple.jsp?pkgname=com.cfastech.yonghu";
					}
				} else {
					//android端
					if (isAndroid) {
						//安卓app的scheme协议
						window.location.href = 'emgoth://';
						setTimeout(function(){
							let hidden = window.document.hidden || window.document.mozHidden || window.document.msHidden ||window.document.webkitHidden 
							if(typeof hidden =="undefined" || hidden ==false){
								//应用宝下载地址 (emmm 找不到淘宝应用宝的地址，这里放的是 lucky coffee 地址)
								window.location.href ="https://a.app.qq.com/o/simple.jsp?pkgname=com.cfastech.yonghu";
							}
						}, 2000);
					}
					
					//ios端
					if (isIOS) {
						//ios的scheme协议
						window.location.href = 'emgoth://'
						setTimeout(function(){
							let hidden = window.document.hidden || window.document.mozHidden || window.document.msHidden ||window.document.webkitHidden 
							if(typeof hidden =="undefined" || hidden ==false){
								//App store下载地址
								window.location.href = "itms-apps://itunes.apple.com/cn/app/%E6%83%A0%E4%BC%97%E5%AE%9D/id1541279288";
							}
						}, 2000);
					}
				} 
			 },
			 
			 
		}
	}
	
	// 兼容
		var browser = {
			versions: function() {
				var u = navigator.userAgent,
					app = navigator.appVersion;
				return {
					trident: u.indexOf('Trident') > -1,
					/*IE内核*/
					presto: u.indexOf('Presto') > -1,
					/*opera内核*/
					webKit: u.indexOf('AppleWebKit') > -1,
					/*苹果、谷歌内核*/
					gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1,
					/*火狐内核*/
					mobile: !!u.match(/AppleWebKit.*Mobile.*/),
					/*是否为移动终端*/
					ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),
					/*ios终端*/
					android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1,
					/*android终端或者uc浏览器*/
					iPhone: u.indexOf('iPhone') > -1,
					/*是否为iPhone或者QQHD浏览器*/
					iPad: u.indexOf('iPad') > -1,
					/*是否iPad*/
					webApp: u.indexOf('Safari') == -1,
					/*是否web应该程序，没有头部与底部*/
					souyue: u.indexOf('souyue') > -1,
					superapp: u.indexOf('superapp') > -1,
					weixin: u.toLowerCase().indexOf('micromessenger') > -1,
					Safari: u.indexOf('Safari') > -1
				};
			}(),
			language: (navigator.browserLanguage || navigator.language).toLowerCase()
		};
		
</script>

<style lang="scss" scoped>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 40rpx;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 100rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}
	
	.title {
		font-size: 30rpx;
		color: $u-content-color;
	}
	
	.button-demo {
		margin-top: 90rpx;
	}
	
	.company-info {
		margin-top: 550rpx;
	}
	.info{
		font-size: 25rpx;
		color: $u-content-color;
	}
</style>
