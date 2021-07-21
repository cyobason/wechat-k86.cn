<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.2;">地区：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="diqu" @click="showDiQu = true" type="select"
								placeholder="请选择所在地区"></u-input>
							<u-select v-model="showDiQu" mode="mutil-column-auto" :list="diquList"
								@confirm="changeDiQu">
							</u-select>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.2;">行业词：</view>
						<view class="soInputValue">
							<u-input maxlength="10" :border="true" v-model="hangye"
								placeholder="行业词, 如: 家具,软件,科技,女装,五金,酒水,餐厅,奶茶等等"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.2;">字数：</view>
						<view class="soInputValue">
							<u-number-box v-model="zi" :min="2" :max="4"></u-number-box>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.2;">专用字：</view>
						<view class="soInputValue">
							<u-switch v-model="zhuan" active-value="1" inactive-value="0" @change="changeZhuan">
							</u-switch>
						</view>
					</view>
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
							<u-parse :html="v.ming_zi"></u-parse>
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
				api_name: '公司起名',
				api_url: app.globalData.api_url,
				diqu: '惠州',
				hangye: '',
				zi: 2,
				zhuan: 1,
				top: 0,
				showDiQu: false,
				diquList: [{
						value: 1,
						label: '广东',
						children: [{
								value: 2,
								label: '惠州'
							},
							{
								value: 3,
								label: '深圳'
							},
							{
								value: 4,
								label: '广州'
							}
						]
					},
					{
						value: 5,
						label: '广西',
						children: [{
								value: 6,
								label: '南宁'
							},
							{
								value: 7,
								label: '桂林'
							}
						]
					}
				],
				winH: app.globalData.winH - 164,
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
				if (!that.hangye) {
					that.showMsg('请输入行业词');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/qi_ming/gong_si', {
					industry: that.hangye,
					san: that.zi,
					limit: that.zhuan
				}, res => {
					that.list = res.list;
					that.list.forEach((v, i) => {
						that.list[i].ming_zi = that.diqu + '<view class="c-error">' + v.ming_zi +
							'</view>' + that.hangye + '有限公司';
					});
				});
			},
			changeDiQu(e) {
				this.diqu = e[1].label;
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
