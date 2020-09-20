<template>
	<view :style="pageIndex==1?'background-color: black;':''" class="content">
		<view class="flexJustifyContentCenter" v-show="pageIndex==0">
			<view class="verticalMiddle">
				<input class="inputStyle" v-model="iptValue" type="text">
				<view>
					文字大小：{{showFontSize}}
					<slider block-size="28" max="200" :value="showFontSize" @changing="sliderChange" step="1" />
				</view>
				<button @click="btnConfirm()" class="blueBtn">确定</button>
			</view>
		</view>
		<view @click="btnCancel()" class="flexJustifyContentCenter" style="height:100%;color:white;" v-show="pageIndex==1">
				<text :style="'font-size:'+showFontSize+'px'">{{iptValue}}</text>
			<!-- <view :style="'font-size:'+showFontSize+'px;'">{{iptValue}}</view> -->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showFontSize:80,
				pageIndex: 0,
				iptValue: '很长的测试文字',
			}
		},
		onLoad() {

		},
		methods: {
			sliderChange(e){
				this.showFontSize=e.detail.value;
			},
			
			btnCancel(){
				this.pageIndex=0
				// #ifdef APP-PLUS
				// plus.screen.
				// #endif
			},
			btnConfirm() {
				this.pageIndex = 1;
				// #ifdef APP-PLUS
				plus.screen.lockOrientation("landscape-primary");
				// #endif
				console.log("confirm")
			}
		}
	}
</script>

<style>
	.inputStyle {
		height: 40px;
		padding-left: 12px;
		border: 1px solid #007AFF;
		margin-bottom: 38px;
	}

	.verticalMiddle {
		position: absolute;
		width: 70%;
		top: 34%;
	}

	.content {
		height: 100%;
		position: fixed;
		width: 100%;
	}
</style>
