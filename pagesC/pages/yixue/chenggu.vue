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
				<block v-if="list.result.zhu_jie == '女性无此命理。'">
					<view class="soListTitle">
						{{list.result.zhu_jie}}
					</view>
				</block>
				<block v-else>
					<view class="soListTitle l-b-t-primary">阳历</view>
					<view class="soListContent c-info">
						{{list.info.yang_li}}
					</view>
					<view class="soListTitle l-b-t-primary">农历</view>
					<view class="soListContent c-info">
						{{list.info.nong_li}}
					</view>
					<view class="soListTitle l-b-t-primary">生肖</view>
					<view class="soListContent c-info">
						{{list.info.sheng_xiao}}
					</view>
					<block v-if="list.result.cheng_gu_ge">
						<view class="soListTitle l-b-t-primary">称骨歌</view>
						<view class="soListContent c-info">
							{{list.result.cheng_gu_ge}}
						</view>
					</block>
					<block v-if="list.result.zhu_jie">
						<view class="soListTitle l-b-t-primary">注解</view>
						<view class="soListContent c-info">
							{{list.result.zhu_jie}}
						</view>
					</block>
					<view class="soListTitle l-b-t-primary">财运</view>
					<view class="soListContent c-info">
						{{list.result.cai_yun}}
					</view>
					<view class="soListTitle l-b-t-primary">福气</view>
					<view class="soListContent c-info">
						{{list.result.fu_qi}}
					</view>
					<view class="soListTitle l-b-t-primary">格局</view>
					<view class="soListContent c-info">
						{{list.result.ge_ju}}
					</view>
					<view class="soListTitle l-b-t-primary">骨重</view>
					<view class="soListContent c-info">
						{{list.result.gu_zhong}}
					</view>
					<view class="soListTitle l-b-t-primary">婚龄</view>
					<view class="soListContent c-info">
						{{list.result.hun_ling}}
					</view>
					<view class="soListTitle l-b-t-primary">命格</view>
					<view class="soListContent c-info">
						{{list.result.ming_ge}}
					</view>
					<view class="soListTitle l-b-t-primary">寿元</view>
					<view class="soListContent c-info">
						{{list.result.shou_yuan}}
					</view>
				</block>
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
				api_name: '称骨算命',
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
				showWin:false,
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
				that.post('sm_cx/cheng_gu', {
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
