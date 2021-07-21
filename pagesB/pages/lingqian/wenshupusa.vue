<template>
	<view class="page bg-white">
		<view class="chouqianBox">
			<view class="chouqianImg">
				<image :src="qiantong" mode="aspectFit"></image>
			</view>
			<view class="chouqianBtn">
				<u-button type="warning" :disabled="disabled" @click="getChouQian">开始抽签</u-button>
			</view>
		</view>
		<view class="tips-warning">
			<u-parse :html="html"></u-parse>
		</view>
		<u-popup v-model="showWin" v-if="Object.keys(list).length > 0" border-radius="20" mode="center" width="95%"
			:closeable="true">
			<view class="qianBox">
				<view class="qianTitle u-text-center">{{list.qian}} {{list.ji_xiong}}</view>
				<view class="qianTitle l-b-t-primary">签文</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_shi)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">解签</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.jie_yue)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">圣意</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.sheng_yi)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">诗文解译</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_shi_jie_yi)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">本签重点</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.ben_qian_zhong_dian)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">爱情婚姻</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.ai_qing_hun_yin)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">求职工作</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qiu_zhi_gong_zuo)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">创业事业</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.chuang_ye_shi_ye)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">考试竞升</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.kao_shi_jing_sheng)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">投资</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.tou_zi)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">经商</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.jing_shang)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">房地买卖</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.fang_di_mai_mai)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">治病</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.zhi_bing)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">转变</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.zhuan_bian)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">求孕</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qiu_yun)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">诉讼运途</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.su_song_yun_tu)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">签诗故事</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_shi_gu_shi)"></u-parse>
				</view>
				<view class="qianTitle l-b-t-primary">签诗故事内容</view>
				<view class="qianContent">
					<u-parse :html="nl2br(list.qian_shi_gu_shi_nei_rong)"></u-parse>
				</view>
			</view>
		</u-popup>
	</view>
</template>

<script>
	const app = getApp();
	export default {
		data() {
			return {
				html:'',
				api_name: '文殊菩萨灵签',
				api_url: app.globalData.api_url,
				disabled: false,
				showWin: false,
				qiantong: app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.png',
				list: []
			}
		},
		onLoad() {
			//获取当前功能信息和配置分享信息
			this.getApi(this.api_name);
		},
		methods: {
			getChouQian() {				
				let that = this;
				that.qiantong = app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.gif';
				that.disabled = true;
				//延迟2秒显示签文
				setTimeout(() => {
					//开始查询
					that.post('ling_qian/wen_shu_pu_sa', {}, res => {
						that.disabled = false;
						that.showWin = true;
						that.qiantong = app.globalData.api_url + '/uploads/xcx/lingqian/qiantong.png';
						that.list = res;
					});
				}, 2000);
			}
		}
	}
</script>

<style>

</style>
