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
				</u-col>
				<u-col span="3">
					<view class="soBtn bg-primary u-m-b-18" @click="getList">查询</view>
				</u-col>
			</u-row>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="soList">
				<view class="soListTitle l-b-t-primary">姓名</view>
				<view class="soListContent c-info">
					{{list.xing.join(' ')}} {{list.ming.join(' ')}}
				</view>
				<view class="soListTitle l-b-t-primary">拼音</view>
				<view class="soListContent c-info">
					{{list.pin_yin.xing.join(' ')}} {{list.pin_yin.ming.join(' ')}}
				</view>
				<view class="soListTitle l-b-t-primary">笔画</view>
				<view class="soListContent c-info">
					{{list.pin_yin.bi_hua.xing.join('画 ')}}画 {{list.pin_yin.bi_hua.ming.join('画 ')}}画
				</view>
				<view class="soListTitle l-b-t-primary">五格</view>
				<view class="soListContent c-info">
					天格：{{list.wu_ge.tian}}（{{list.san_cai_wu_xing.tian}}）
				</view>
				<view class="soListContent c-info">
					人格：{{list.wu_ge.ren}}（{{list.san_cai_wu_xing.ren}}）
				</view>
				<view class="soListContent c-info">
					地格：{{list.wu_ge.di}}（{{list.san_cai_wu_xing.di}}）
				</view>
				<view class="soListContent c-info">
					外格：{{list.wu_ge.wai}}（{{list.san_cai_wu_xing.wai}}）
				</view>
				<view class="soListContent c-info">
					总格：{{list.wu_ge.zong}}（{{list.san_cai_wu_xing.zong}}）
				</view>
				<view class="soListTitle l-b-t-primary">三才</view>
				<block v-for="(v,i) in list.san_cai_zi_duan" :key="i">
					<view class="soListContent c-primary bold">{{v}}：</view>
					<view class="soListContent c-info" v-for="(vv,ii) in list.san_cai[i]" :key="ii">
						{{vv}}
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
				api_name: '五格三才',
				api_url: app.globalData.api_url,
				xing: '',
				ming: '',
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
				//开始查询
				that.post('sm_cx/wu_ge_san_cai', {
					xing: that.xing,
					ming: that.ming
				}, res => {
					that.showWin = true;
					that.list = res;
				});
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
