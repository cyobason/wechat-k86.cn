<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">生日：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen" @click="showShiChen = true" type="select"
								placeholder="请选择您的阳历生日"></u-input>
							<u-picker v-model="showShiChen" mode="time" :params="shichenList" @confirm="changeShiChen">
							</u-picker>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">性别：</view>
						<view class="soInputValue">
							<view class="u-m-r-10 c-info">女</view>
							<u-switch v-model="sex" active-value="1" inactive-value="0" @change="changeSex">
							</u-switch>
							<view class="u-m-l-10 c-info">男</view>
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
				<view class="soListTitle l-b-t-primary">大运流年</view>
				<view class="soListContent c-info">					
					<view class="soListContent c-info u-m-b-20">出生后{{list.da_yun_liu_nian.qi_yun.sui}}岁开始起大运，即{{list.da_yun_liu_nian.qi_yun.nian}}年开始交运，每十年进入下一步运。</view>
					<u-table>
						<u-tr>
							<u-th>十神</u-th>
							<u-th>大运</u-th>
							<u-th>年份</u-th>
							<u-th>岁数</u-th>
						</u-tr>
						<u-tr v-for="(v,i) in list.da_yun_liu_nian.liu_nian" :key="i">
							<u-td>{{v.shi_chen}}</u-td>
							<u-td>{{v.gan_zhi}}</u-td>
							<u-td>{{v.nian}}</u-td>
							<u-td>{{v.sui}}岁</u-td>
						</u-tr>						
					</u-table>
				</view>
				<view class="soListTitle l-b-t-primary">近十个月运程</view>
				<view class="soListContent c-info" v-for="(v,i) in list.jin_shi_ge_yue" :key="i">
					<u-line class="u-line u-m-t-20 u-m-b-20" border-style="dashed" v-if="i != 0" :hair-line="false">
					</u-line>
					<view class="soListContent c-primary">{{v.yue}}</view>
					<view class="soListContent c-info">
						<u-parse :html="nl2br(v.shi_ju)"></u-parse>
					</view>
					<view class="soListContent c-info">
						<text class="c-success">十神为{{v.shi_shen}}：</text>
						<u-parse :html="nl2br(v.ji_xiong)"></u-parse>
					</view>
				</view>
				<view class="soListTitle l-b-t-primary">近十年运程</view>
				<view class="soListContent c-info" v-for="(v,i) in list.shi_nian_yun_cheng" :key="i">
					<u-line class="u-line u-m-t-20 u-m-b-20" border-style="dashed" v-if="i != 0" :hair-line="false">
					</u-line>
					<view class="soListContent c-primary">{{v.kai_shi}} ~ {{v.jie_shu}}</view>
					<view class="soListContent c-info">
						<u-parse :html="nl2br(v.shi_ju)"></u-parse>
					</view>
					<view class="soListContent c-info">
						<text class="c-success">十神为{{v.shi_shen}}：</text>
						<u-parse :html="nl2br(v.ji_xiong)"></u-parse>
					</view>
				</view>
				<view class="soListTitle l-b-t-primary">命格</view>
				<view class="soListContent c-info">
					<u-parse :html="nl2br(list.ming_ge)"></u-parse>
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
				api_name: '八字运程',
				api_url: app.globalData.api_url,
				sex: 0,
				shichen: '',
				showShiChen: false,
				shichenList: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: true,
					second: false,
					timestamp: true
				},
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
				if (!that.shichen) {
					that.showMsg('请选择您的阳历生日');
					return false;
				}
				//开始查询
				that.post('sm_cx/ba_zi_yun_cheng', {
					sex: that.sex,
					date: that.shichen,
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen(e) {
				this.shichen = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute;
			},
			changeSex(e) {
				this.sex = e;
			}
		}
	}
</script>

<style>
	
</style>
