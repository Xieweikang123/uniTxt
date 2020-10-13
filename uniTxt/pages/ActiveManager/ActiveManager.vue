<template>
	<view>
		<view>用户uuid</view>
		<input v-model="uuid" />
		<button @click="activeBtn()"  style="margin-top: 26px;width: 74%;" type="primary">激活</button>
		<view>生成结果</view>
		<input v-model="activeCode" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				activeCode:'',
				uuid: "990009269293679,990009269593672"
			}
		},
		onLoad() {
			 
		},
		methods: {
			activeBtn(){
				this.activeCode=this.compile(this.uuid)
			},
			 compile(code) {
				var c = String.fromCharCode(code.charCodeAt(0) + code.length);
				for (var i = 1; i < code.length; i++) {
					c += String.fromCharCode(code.charCodeAt(i) + code.charCodeAt(i - 1));
				}
				return (escape(c));
			},

			 uncompile(code) {
				code = unescape(code);
				var c = String.fromCharCode(code.charCodeAt(0) - code.length);
				for (var i = 1; i < code.length; i++) {
					c += String.fromCharCode(code.charCodeAt(i) - c.charCodeAt(i - 1));
				}
				return c;
			}
		}
	}
</script>

<style>

</style>
