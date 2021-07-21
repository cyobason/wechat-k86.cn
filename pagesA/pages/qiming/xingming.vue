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
							<u-input :border="true" v-model="ming" placeholder="请输入您的名字"></u-input>
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
				<view class="soListTitle l-b-t-primary">综合分析</view>
				<u-row gutter="16">
					<u-col span="9">
						<view class="soListTitle">
							{{list.xing_ming.xing_ming}}（{{list.xing_ming.pin_yin.xing.join(' ') + ' ' + list.xing_ming.pin_yin.ming.join(' ')}}）
						</view>
					</u-col>
					<u-col span="3">
						<view class="u-flex u-row-right">
							<block v-for="i in list.xing_ming.de_fen.length" :key="i">
								<image src="/static/images/stars.png" mode="aspectFit"
									style="width:30rpx;height: 30rpx;margin-right: 5rpx;"></image>
							</block>
						</view>
					</u-col>
				</u-row>
				<u-row gutter="16" class="u-m-b-10" v-if="list.xing_ming.cheng_yu > 0 || list.xing_ming.shi_ci > 0">
					<u-col span="12">
						<u-tag text="诗词" size="mini" mode="light" class="u-m-r-10" v-if="list.xing_ming.shi_ci" />
						<u-tag text="成语" size="mini" type="success" mode="light" v-if="list.xing_ming.cheng_yu" />
					</u-col>
				</u-row>
				<u-row gutter="16" v-for="(v,i) in list.xing_ming.han_yi" :key="i" class="u-m-b-10">
					<u-col span="9">
						<view class="u-font-25 c-info cut">{{v.zi}}:（{{list.xing_ming.pin_yin.ming[i]}}）{{v.yi}}</view>
					</u-col>
					<u-col span="3">
						<view class="u-flex u-row-right c-warning" v-if="i == 0">
							{{list.xing_ming.nan_nv}}
						</view>
						<view class="u-flex u-row-right c-info" v-else>
							重名率{{list.xing_ming.chong_ming}}
						</view>
					</u-col>
				</u-row>
				<u-row gutter="16" class="u-m-b-10">
					<u-col span="12">
						<block v-for="(v,i) in list.xing_ming.zi_yi" :key="i">
							<u-tag :text="v" size="mini" mode="light" type="warning" class="u-m-r-10" />
						</block>
					</u-col>
				</u-row>
				<view class="soListTitle l-b-t-primary">谐音字</view>
				<view class="soListContent c-info" v-for="(v,i) in list.zi_yin.xie_yin" :key="i">
					<text class="c-warning">{{v.ci}}：</text>
					<u-parse :html="nl2br(v.jie_shi)"></u-parse>
				</view>
				<view class="soListTitle l-b-t-primary">五格</view>
				<view class="soListContent c-info">天格：{{list.wu_ge.ge_shu.tian}}（{{list.wu_ge.wu_xing.tian}}）</view>
				<view class="soListContent c-primary">{{list.wu_ge.shu_li.tian}}</view>
				<view class="soListContent c-info">人格：{{list.wu_ge.ge_shu.ren}}（{{list.wu_ge.wu_xing.ren}}）</view>
				<view class="soListContent c-primary">{{list.wu_ge.shu_li.ren}}</view>
				<view class="soListContent c-info">地格：{{list.wu_ge.ge_shu.di}}（{{list.wu_ge.wu_xing.di}}）</view>
				<view class="soListContent c-primary">{{list.wu_ge.shu_li.di}}</view>
				<view class="soListContent c-info">外格：{{list.wu_ge.ge_shu.wai}}（{{list.wu_ge.wu_xing.wai}}）</view>
				<view class="soListContent c-primary">{{list.wu_ge.shu_li.wai}}</view>
				<view class="soListContent c-info">总格：{{list.wu_ge.ge_shu.zong}}（{{list.wu_ge.wu_xing.zong}}）</view>
				<view class="soListContent c-primary">{{list.wu_ge.shu_li.zong}}</view>
				<view class="soListTitle l-b-t-primary">三才</view>
				<view class="soListContent c-info">{{list.wu_ge.san_cai}}</view>				
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
				api_name: '姓名分析',
				api_url: app.globalData.api_url,
				xing: '',
				ming: '',
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
				if (!that.xing) {
					that.showMsg('请输入您的姓氏');
					return false;
				}
				if (!that.ming) {
					that.showMsg('请输入您的名字');
					return false;
				}
				if (!that.shichen) {
					that.showMsg('请选择您的阳历生日');
					return false;
				}
				//回到顶部
				that.top = Math.random();
				//开始查询
				that.post('/qi_ming/xing_ming', {
					xing: that.xing,
					ming: that.ming,
					sex: that.sex,
					date: that.shichen
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
	/* 重写样式 */
	.soBtn {
		height: 164rpx;
	}

	.soInputValue {
		flex: 4;
		display: flex;
		align-items: center;
	}
</style>
