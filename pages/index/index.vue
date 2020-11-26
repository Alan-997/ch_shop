<template>
	<view class="home">
		<swiper-list :swipers="swipers"></swiper-list>
		<view class="main-nav">
			<view class="nav-item" 
						v-for="(item, index) in navItem" :key="index" 
						@click="navItemClick(item.path)">
				<view :class="item.class"></view>
				<text>{{item.title}}</text>
			</view>
		</view>
		
		<!-- 推荐商品 -->
		<view class="rmd-goods">
			<view class="rmd-bar">
				<text>推荐商品</text>
			</view>
			<view class="goods">
				<goods-list :goods="goods"></goods-list>
			</view>
		</view>
		
	</view>
</template>

<script>
	import goodsList from "../../components/goodslist/goodsList.vue"
	import swiperList from "../../components/swiper/swiperList.vue"
	export default {
		data() {
			return {
				swipers: [],
				goods: [],
				pageindex: 1,
				navItem: [
					{
						class: 'iconfont icon-ziyuan',
						title: '爱懒超市',
						path: '../../pages/goods/goods',
					},
					{
						class: 'iconfont icon-guanyuwomen',
						title: '联系爱懒',
						path: '../../pages/about/about',
					},
					{
						class: 'iconfont icon-tupian',
						title: '社区图片',
						path: '../../pages/community/community',
					},
					{
						class: 'iconfont icon-shipin',
						title: '学习视频',
						path: '../../pages/video/video',
					}
				]
			}
		},
		components: {
			goodsList,
			swiperList
		},
		onLoad() {
			this.getSwipers(),
			this.getGoods()
		},
		methods: {
			getSwipers() {
				uni.request({
					url:"http://localhost:8082/api/getlunbo",
					success: res => {
						console.log(res)
						if(res.data.status !== 0){
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.swipers = res.data.message
					}
				})
			},
			getGoods() {
				uni.request({
					url:"http://localhost:8082/api/getgoods",
					success: res => {
						console.log(res)
						if(res.data.status !== 0){
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.goods = res.data.message
					}
				})
			},
			navItemClick (url) {
				uni.navigateTo({
					url
				})
			}
			
			
		}
	}
</script>

<style lang="scss">
	.home{
		.main-nav {
			display: flex;
			justify-content: space-around;
			padding: 20px 0;
			font-size: 16px;
			.nav-item {
				text-align: center;
				.iconfont {
					width: 120rpx;
					height: 120rpx;
					line-height: 120rpx;
					color: #fff;
					margin: 0 auto;
					border-radius: 50%;
					background-color: #B50E03;
					margin-bottom: 10px;
					font-size: 24px;
				}
			}
		}
		.rmd-goods {
			background-color: #eee;
			padding-top: 10rpx;
			.rmd-bar {
				text-align: center;
				font-size: 24px;
				color: #B50E03;
				background-color: #fff;
				letter-spacing:14px;
				padding: 10px 0;
			}
			.goods {
				margin-top: 10rpx;
			}
		}
		
		
	}
</style>
