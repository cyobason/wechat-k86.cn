<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel">姓氏：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="xing" placeholder="请输入您的姓氏"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">名字：</view>
						<view class="soInputValue">
							<u-input maxlength="1" :border="true" v-model="ming" placeholder="请输入名字里面包含的字"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel">生日：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen" @click="showShiChen = true" type="select"
								placeholder="请选择您的阳历生日"></u-input>
							<u-picker v-model="showShiChen" mode="time" :params="shichenList" @confirm="changeShiChen">
							</u-picker>
						</view>
					</view>
					<u-row gutter="0">
						<u-col span="6">
							<view class="soInput">
								<view class="soInputLabel" style="flex: 1.7;">单字：</view>
								<view class="soInputValue" style="flex: 3;">
									<u-switch v-model="dan" active-value="1" inactive-value="0" @change="changeDan">
									</u-switch>
								</view>
							</view>
						</u-col>
						<u-col span="6">
							<view class="soInput">
								<view class="soInputLabel" style="flex: 2.7;">专用字：</view>
								<view class="soInputValue" style="flex: 3;">
									<u-switch v-model="zhuan" active-value="1" inactive-value="0" @change="changeZhuan">
									</u-switch>
								</view>
							</view>
						</u-col>
					</u-row>
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<scroll-view scroll-y :style="'height:'+ winH +'px;'" :scroll-top="top" v-if="list.length > 0">
			<view class="soList" v-for="(v,i) in list" :key="i">
				<u-row gutter="16">
					<u-col span="9">
						<view class="soListTitle">
							{{v.xing_ming}}（{{v.pin_yin.xing.join(' ') + ' ' + v.pin_yin.ming.join(' ')}}）
						</view>
					</u-col>
					<u-col span="3">
						<view class="u-flex u-row-right">
							<block v-for="i in v.de_fen.length" :key="i">
								<image src="/static/images/stars.png" mode="aspectFit"
									style="width:30rpx;height: 30rpx;margin-right: 5rpx;"></image>
							</block>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="16" class="u-m-b-10" v-if="v.cheng_yu > 0 || v.shi_ci > 0">
					<u-col span="12">
						<u-tag text="诗词" size="mini" mode="light" class="u-m-r-10" v-if="v.shi_ci" />
						<u-tag text="成语" size="mini" type="success" mode="light" v-if="v.cheng_yu" />
					</u-col>
				</u-row>
				<u-row gutter="16" v-for="(vv,ii) in v.han_yi" :key="ii" class="u-m-b-10">
					<u-col span="9">
						<view class="u-font-25 c-info cut">{{vv.zi}}:（{{v.pin_yin.ming[ii]}}）{{vv.yi}}</view>
					</u-col>
					<u-col span="3">
						<view class="u-flex u-row-right c-warning" v-if="ii == 0">
							{{v.nan_nv}}
						</view>
						<view class="u-flex u-row-right c-info" v-else>
							重名率{{v.chong_ming}}
						</view>
					</u-col>
				</u-row>
				<u-row gutter="16" class="u-m-b-10">
					<u-col span="12">
						<block v-for="(vv,ii) in v.zi_yi" :key="ii">
							<u-tag :text="vv" size="mini" mode="light" type="warning" class="u-m-r-10" />
						</block>
					</u-col>
				</u-row>
			</view>
		</scroll-view>
		<view class="tips-warning" v-else>
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
				api_name: '男宝宝起名',
				api_url: app.globalData.api_url,
				xing: '',
				ming: '',
				dan: 0,
				zhuan: 1,
				top: 0,
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
				winH: app.globalData.winH - 180,
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
				if (!that.xing) {
					that.showMsg('请输入您的姓氏');
					return false;
				}
				if (!that.shichen) {
					that.showMsg('请选择您的阳历生日');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/qi_ming/bao_bao', {
					xing: that.xing,
					zi: that.ming,
					sex: 1,
					dan: that.dan,
					shiChen: that.shichen,
					limit: that.zhuan
				}, res => {
					that.list = res.list;
				});
			},
			changeShiChen(e) {
				this.shichen = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute;
			},
			changeDan(e) {
				this.dan = e;
			},
			changeZhuan(e) {
				this.zhuan = e;
			}
		}
	}
</script>

<style>
	/* 重写样式 */
	.soBtn {
		height: 164rpx;
	}
</style>
