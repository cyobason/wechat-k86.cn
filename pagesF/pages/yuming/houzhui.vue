<template>
	<view class="page">
		<view class="tips-warning">
			<u-parse :html="html"></u-parse>
		</view>
		<u-table>
			<u-tr>
				<u-th>后缀</u-th>				
			</u-tr>
			<u-tr v-for="(v,i) in list" :key="i">				
				<u-td>{{v}}</u-td>
			</u-tr>
		</u-table>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				html: '',
				api_name: '域名后缀',
				api_url: app.globalData.api_url,
				list: [
					".aaa.pro",
					".abogado",
					".ac",
					".ac.mu",
					".ac.nz",
					".aca.pro",
					".academy",
					".accountant",
					".accountants",
					".acct.pro",
					".actor",
					"...",
				]
			}
		},
		onLoad() {
			//获取当前功能信息和配置分享信息
			this.getApi(this.api_name);
		},
		methods: {}
	}
</script>

<style>
	
</style>
