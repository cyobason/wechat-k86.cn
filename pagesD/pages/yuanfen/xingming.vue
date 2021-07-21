<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">男方姓名：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="nan" placeholder="请选择男方姓名"></u-input>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">女方姓名：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="nv" placeholder="请选择女方姓名"></u-input>
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
				<view class="soListTitle l-b-t-primary">算法1</view>				
				<view class="soListContent c-info" v-for="(v,i) in list.one" :key="i">
					{{v}}
				</view>
				<view class="soListTitle l-b-t-primary">算法2</view>
				<view class="soListContent c-info" v-for="(v,i) in list.two" :key="i">
					{{v}}
				</view>
				<view class="soListTitle l-b-t-primary">算法3</view>
				<view class="soListContent c-info">
					{{list.three}}
				</view>
				<view class="soListTitle l-b-t-primary">算法4</view>
				<view class="soListContent c-info">
					{{list.four}}
				</view>
				<view class="soListTitle l-b-t-primary">算法5</view>
				<view class="soListContent c-info">
					{{list.five}}
				</view>
				<view class="soListTitle l-b-t-primary">性格</view>
				<view class="soListContent c-primary bold">男方</view>
				<view class="soListContent c-info" v-for="(v,i) in list.xing_ge.nan" :key="i">
					{{v}}
				</view>
				<view class="soListContent c-primary bold">女方</view>
				<view class="soListContent c-info" v-for="(v,i) in list.xing_ge.nv" :key="i">
					{{v}}
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
				api_name: '姓名配对',
				api_url: app.globalData.api_url,
				nan: '',
				nv: '',
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
				if (!that.nan) {
					that.showMsg('请选择男方姓名');
					return false;
				}
				if (!that.nv) {
					that.showMsg('请选择女方姓名');
					return false;
				}
				//开始查询
				that.post('yuan_fen/xing_ming', {
					nan: that.nan,
					nv: that.nv
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
