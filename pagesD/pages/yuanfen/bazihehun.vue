<template>
	<view class="page">
		<view class="soBox">
			<u-row gutter="16">
				<u-col span="9">
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">男方生日：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen1" @click="showShiChen1 = true" type="select"
								placeholder="请选择男方阳历生日"></u-input>
							<u-picker v-model="showShiChen1" mode="time" :params="shichenList"
								@confirm="changeShiChen1">
							</u-picker>
						</view>
					</view>
					<view class="soInput">
						<view class="soInputLabel" style="flex: 1.5;">女方生日：</view>
						<view class="soInputValue">
							<u-input :border="true" v-model="shichen2" @click="showShiChen2 = true" type="select"
								placeholder="请选择女方阳历生日"></u-input>
							<u-picker v-model="showShiChen2" mode="time" :params="shichenList"
								@confirm="changeShiChen2">
							</u-picker>
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
				<view class="soListTitle l-b-t-primary">命宫</view>
				<view class="soListContent c-error">{{list.he_hun.ming_gong}}分</view>
				<view class="soListContent c-info">此项为30分。说明：以东四命与西四命之说来合，如果相合，那么在购房时，应买与自己命宫相合的房子。</view>
				<view class="soListTitle l-b-t-primary">年支同气</view>
				<view class="soListContent c-error">{{list.he_hun.nian_zhi}}分</view>
				<view class="soListContent c-info">此项为20分。说明：如寅卯辰会东方木气，虎兔龙结合的机缘就大于其它属相；巳午未会南方火气，蛇马羊结合的机缘就大于其它属相；申酉戌会西方金气，猴鸡狗结合的机缘就大于其它属相；亥子丑会北方水气，猪鼠牛结合的机缘就大于其它属相。</view>
				<view class="soListTitle l-b-t-primary">月令合</view>
				<view class="soListContent c-error">{{list.he_hun.yue_ling}}分</view>
				<view class="soListContent c-info">此项为5分。说明：男女生月相同者互相间也是很有缘份的。</view>
				<view class="soListTitle l-b-t-primary">日干相合</view>
				<view class="soListContent c-error">{{list.he_hun.ri_gan}}分</view>
				<view class="soListContent c-info">此项为25分。说明：何谓日干舒配得所？日干五行相同，一阴一阳的组合男女结合的机缘最大，如甲日干逢乙日干，庚日干逢辛日干之类。</view>
				<view class="soListTitle l-b-t-primary">天干五合</view>
				<view class="soListContent c-error">{{list.he_hun.tian_gan}}分</view>
				<view class="soListContent c-info">此项为5分。说明：其次是天干五合，如甲日干逢己日干，庚日干逢乙日干之类。再次则是比和或相生。</view>
				<view class="soListTitle l-b-t-primary">子女同步</view>
				<view class="soListContent c-error">{{list.he_hun.zi_nv}}分</view>
				<view class="soListContent c-info">此项为15分。说明：何谓子女同步？西方的科学家在探索男女结合的奥秘时提出了 " 性染色体论 " ，我们东方人在四柱预测中看头胎子女的性别，男女双方的八字中头胎子女的性别必须一致。</view>
				<view class="soListTitle l-b-t-primary">得分</view>
				<view class="soListContent c-error">{{list.de_fen}}分</view>				
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
				api_name: '八字合婚',
				api_url: app.globalData.api_url,
				shichen1: '',
				showShiChen1: false,
				shichenList: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: true,
					second: false,
					timestamp: true
				},
				shichen2: '',
				showShiChen2: false,
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
				if (!that.shichen1) {
					that.showMsg('请选择男方阳历生日');
					return false;
				}
				if (!that.shichen2) {
					that.showMsg('请选择女方阳历生日');
					return false;
				}
				//开始查询
				that.post('yuan_fen/ba_zi_he_hun', {
					nan: that.shichen1,
					nv: that.shichen2
				}, res => {
					that.showWin = true;
					that.list = res;
				});
			},
			changeShiChen1(e) {
				this.shichen1 = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute;
			},
			changeShiChen2(e) {
				this.shichen2 = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':' + e.minute;
			},
		}
	}
</script>

<style>
	/* 重写样式 */
	.soBtn {
		height: 164rpx;
	}
</style>
