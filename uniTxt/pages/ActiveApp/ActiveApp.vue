<template>
	<view style="padding:10px 20px">
		<view>设备id</view>
		<input v-model="uuid"></input>
		<view>激活码</view>
		<input style="border: 1px solid #808080;" v-model="activeCode" />
		<button @click="activeBtn()"  style="margin-top: 26px;width: 74%;" type="primary">激活</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				activeCode:'Xri%60%60ikhokklimpeeri%60%60ikhonnlimi',
				uuid:'未获取到uuid'
			}
		},
		onLoad() {
			 uni.getStorage({
				key: 'isActiveApp',
				success: function(res) {
					console.log("isactive")
					uni.navigateTo({
						url:"../SimulationCallRecords/SimulationCallRecords"
					})
				}
				});
				
			this.getDeviceInfo()
		},
		methods: {
			activeBtn(){
				if(this.activeCode.length==0){
					uni.showToast({
						icon:'none',
						title:"请输入激活码"
					})
				}
				//如果解密出来的是uuid 成功
				if(this.uncompile(this.activeCode)==this.uuid){
					uni.setStorage({
					    key: 'isActiveApp',
					    data: true
					   });
					uni.navigateTo({
						url:"../SimulationCallRecords/SimulationCallRecords"
					})
				}else{
					uni.showToast({
						icon:'none',
						title:"激活码错误"
					})
				}
				
			
			},uncompile(code) {
				code = unescape(code);
				var c = String.fromCharCode(code.charCodeAt(0) - code.length);
				for (var i = 1; i < code.length; i++) {
					c += String.fromCharCode(code.charCodeAt(i) - c.charCodeAt(i - 1));
				}
				return c;
			},
			// #ifdef APP-PLUS
			
			// 获取设备信息
			getDeviceInfo(){
				var that=this;
				plus.device.getInfo({
					success:function(e){
						console.log('getDeviceInfo success: ',e);
						that.uuid=e.uuid;
					},
					fail:function(e){
						console.log('getDeviceInfo failed: '+JSON.stringify(e));
					}
				});
			},
			// #endif
		}
	}
</script>

<style>

</style>
