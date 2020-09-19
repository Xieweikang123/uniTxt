<template>
	<view :style="pageIndex==1?'background-color: black;':''" class="content">
		<div class="flexJustifyContentCenter" v-show="pageIndex==0">
			<div class="verticalMiddle">
				<input class="inputStyle" v-model="iptValue" type="text">
				<button @click="btnConfirm()" class="blueBtn">确定</button>
			</div>
		</div>
		<div @click="pageIndex=0" class="flexJustifyContentCenter" style="height:100%;color:white;" v-show="pageIndex==1">
			{{iptValue}}
			<!-- <div :style="'font-size:'+showFontSize+'px;'">{{iptValue}}</div> -->
		</div>
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
			btnConfirm() {
				this.pageIndex = 1;
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
