<template>
	<view class="page">
		<u-cell-group class="u-m-t-20">
			<u-cell-item title="版本号" :value="version" :arrow="false" hover-class="none"></u-cell-item>
		</u-cell-group>
		<u-tabbar :list="tabbar" active-color="#2979ff" inactive-color="#424242"></u-tabbar>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				version: app.globalData.version,
				tabbar: this.tabbar
			}
		},
		onLoad() {},
		methods: {

		}
	}
</script>

<style>

</style>
