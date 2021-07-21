<template>
	<view class="page">
		<view class="tips-warning">
			<u-parse :html="html"></u-parse>
		</view>
		<u-table>
			<u-tr>
				<u-th>简称</u-th>
				<u-th>全称</u-th>
				<u-th>区号</u-th>
			</u-tr>
			<u-tr v-for="(v,i) in list" :key="i">
				<u-td>{{i}}</u-td>
				<u-td>{{v.name}}</u-td>
				<u-td>{{v.phone_cc}}</u-td>
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
				api_name: '地区代码',
				api_url: app.globalData.api_url,
				list: {
					"AD": {
						"name": "Andorra",
						"phone_cc": "376"
					},
					"AE": {
						"name": "United Arab Emirates",
						"phone_cc": "971"
					},
					"AF": {
						"name": "Afghanistan",
						"phone_cc": "93"
					},
					"AG": {
						"name": "Antigua and Barbuda",
						"phone_cc": "1"
					},
					"AI": {
						"name": "Anguilla",
						"phone_cc": "1"
					},
					"AL": {
						"name": "Albania",
						"phone_cc": "355"
					},
					"AM": {
						"name": "Armenia",
						"phone_cc": "374"
					},
					"AO": {
						"name": "Angola",
						"phone_cc": "244"
					},
					"AR": {
						"name": "Argentina",
						"phone_cc": "54"
					},
					"AS": {
						"name": "American Samoa",
						"phone_cc": "1"
					},
					"AT": {
						"name": "Austria",
						"phone_cc": "43"
					},
					"...": {
						"name": "...",
						"phone_cc": "..."
					}
				}
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
