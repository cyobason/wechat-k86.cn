<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">生肖：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shengxiao" @click="showShengXiao = true" type="select"
								placeholder="请选择生肖"></u-input>
							<u-select v-model="showShengXiao" mode="single-column" :list="shengxiaoList"
								@confirm="changeShengXiao"></u-select>
						</view>
					</view>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="soList">
				<view class="soListTitle l-b-t-primary">今年财运</view>
				<view class="soListContent c-info">
					{{list.cai_yun}}
				</view>
				<view class="soListTitle l-b-t-primary">今年吉祥物</view>
				<view class="soListContent c-info">
					{{list.ji_xiang_wu}}
				</view>
				<view class="soListTitle l-b-t-primary">今年感情运</view>
				<view class="soListContent c-info">
					{{list.gan_qing}}
				</view>
				<view class="soListTitle l-b-t-primary">今年健康运</view>
				<view class="soListContent c-info">
					{{list.jian_kang}}
				</view>
				<view class="soListTitle l-b-t-primary">今年事业运</view>
				<view class="soListContent c-info">
					{{list.shi_ye}}
				</view>
				<view class="soListTitle l-b-t-primary">今年总述</view>
				<view class="soListContent c-info">
					{{list.zong_shu}}
				</view>
				<view class="soListTitle l-b-t-primary">今年运程</view>
				<view class="soListContent c-info" v-for="(v,i) in list.yue" :key="i">
					<u-line class="u-line" border-style="dashed" margin="20rpx 0" v-if="i != 0" :hair-line="false"></u-line>
					<view class="soListContent c-primary">
						{{v[0]}}
					</view>
					<view class="soListContent c-error">
						{{v[1]}}
					</view>
					<view class="soListContent c-info">
						{{v[2]}}
					</view>
				</view>
				<view class="soListTitle l-b-t-primary">属{{shengxiao}}年龄</view>
				<view class="soListContent c-info" v-for="(v,i) in list.nian" :key="i">
					{{i}}年：{{v}}
				</view>				
			</view>
		</u-popup>
		<view class="tips-warning">
			<u-parse :html="html"></u-parse>
		</view>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				html:'',
				api_name: '生肖运程',
				api_url: app.globalData.api_url,
				shengxiao: '',
				showShengXiao: false,
				shengxiaoList: [{
						label: '鼠',
						value: '鼠'
					},
					{
						label: '牛',
						value: '牛'
					},
					{
						label: '虎',
						value: '虎'
					},
					{
						label: '兔',
						value: '兔'
					},
					{
						label: '龙',
						value: '龙'
					},
					{
						label: '蛇',
						value: '蛇'
					},
					{
						label: '马',
						value: '马'
					},
					{
						label: '羊',
						value: '羊'
					},
					{
						label: '猴',
						value: '猴'
					},
					{
						label: '鸡',
						value: '鸡'
					},
					{
						label: '狗',
						value: '狗'
					},
					{
						label: '猪',
						value: '猪'
					}
				],
				showWin: false,
				list: []
			}
		},
		onLoad() {
			//获取当前功能信息和配置分享信息
			this.getApi(this.api_name);
		},
		methods: {
			getList() {
				let that = this;
				if (!that.shengxiao) {
					that.showMsg('请选择生肖');
					return false;
				}
				//开始查询
				that.post('sm_cx/sheng_xiao_nian_cheng', {
					shengXiao: that.shengxiao
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShengXiao(e) {
				this.shengxiao = e[0].label;
			}
		}
	}
</script>

<style>

</style>
