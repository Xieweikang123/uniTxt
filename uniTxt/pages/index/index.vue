<template>
	<view class="content">
		<div class="verticalMiddle">
			<input class="inputStyle" v-model="iptValue"  type="text">
			<button >确定</button>
		</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				iptValue:'测试文字',
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.inputStyle{
		    height: 40px;
		    padding-left: 12px;
		border:1px solid #007AFF;
		margin-bottom:38px;
	}
	.verticalMiddle{
		    padding: 0 25px;
		    margin-top: 40%;
	}
	.content {
	    height: 100%;
	    position: absolute;
	    width: 100%;
	}

 
</style>
