<template>
	<view class="details">
		<view class="details_swiper">
			<swiper class="swiper" indicator-dots autoplay circular>
				<swiper-item class="swiper-item" v-for="(item,index) in detailsImg" :key="index">
					<image :src="item.src" mode="" @click="perviewImg(item.src)"></image>
				</swiper-item>
			</swiper>
		</view>
		<view class="details_info" v-for="(item,index) in detailsInfo" :key="index">
			<view class="price">
				<text class="present_price">￥{{item.sell_price}}</text>
				<text class="original_price">￥{{item.market_price}}</text>
			</view>
			<view class="title">
				<text class="">{{item.title}}</text>
			</view>
			<view class="goods_msg">
				<view class="number">
					<text>货号：{{item.goods_no}}</text>
				</view>
				<view class="stock_quantity">
					<text>库存：{{item.stock_quantity}}</text>
				</view>
			</view>
		</view>
		<view class="introduced" v-for="(item, index) in detailsDesc">
			<view class="indd_title">
				<text>详情介绍</text>
			</view>
			<view class="indd_content">
				<rich-text :nodes="item.content"></rich-text>
			</view>
		</view>
		<view class="goods_nav">
			<uni-goods-nav :fill="true" :options="options" :buttonGroup="buttonGroup" @click="onClick" @buttonClick="buttonClick" />
		</view>
	</view>
</template>

<script>
	import uniGoodsNav from '@/components/uni-goods-nav/uni-goods-nav.vue'
	export default {
		data() {
			return {
				id: 0,
				detailsImg: [],
				detailsInfo: [],
				detailsDesc: [],
				showImg: [],
				options: [{
					icon: 'headphones',
					text: '客服'
				}, {
					icon: 'shop',
					text: '店铺',
					info: 2,
					infoBackgroundColor: '#007aff',
					infoColor: "red"
				}, {
					icon: 'cart',
					text: '购物车',
					info: 2
				}],
				buttonGroup: [{
						text: '加入购物车',
						backgroundColor: '#ff0000',
						color: '#fff'
					},
					{
						text: '立即购买',
						backgroundColor: '#ffa200',
						color: '#fff'
					}
				]
			};
		},
		onLoad(options) {
			this.id = options.id
			this.getDetailsImg()
			this.getDetailsInfo()
			this.getDetailsDesc()
		},
		components: {
			uniGoodsNav
		},
		methods: {
			getDetailsImg() {
				uni.request({
					url: "http://localhost:8082/api/getthumimages/" + this.id,
					success: res => {
						// console.log(res)
						if (res.data.status !== 0) {
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.detailsImg = res.data.message
						for (let i = 0; i <= this.detailsImg.length - 1; i++) {
							this.showImg.push(this.detailsImg[i].src)
						}
					}
				})
			},
			getDetailsInfo() {
				uni.request({
					url: "http://localhost:8082/api/goods/getinfo/" + this.id,
					success: res => {
						// console.log(res)
						if (res.data.status !== 0) {
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.detailsInfo = res.data.message
					}
				})
			},
			getDetailsDesc() {
				uni.request({
					url: "http://localhost:8082/api/goods/getdesc/" + this.id,
					success: res => {
						console.log(res)
						if (res.data.status !== 0) {
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.detailsDesc = res.data.message
					}
				})
			},
			perviewImg(current) {
				uni.previewImage({
					current,
					urls: this.showImg,
					loop: true,
					indicator: Number
				})
			},
			onClick(e) {
				uni.showToast({
					title: `点击${e.content.text}`,
					icon: 'none'
				})
			},
			buttonClick(e) {
				console.log(e)
				this.options[2].info++
			}
		}
	}
</script>

<style lang="scss">
	.details {
		.details_swiper {
			.swiper {
				width: 750rpx;
				height: 700rpx;

				.swiper-item {
					text-align: center;

					image {
						width: 750rpx;
						height: 700rpx;
					}
				}
			}
		}

		.details_info {
			.price {
				padding: 10rpx 20rpx;

				.present_price {
					color: #CF635C;
					font-size: 18px;
					margin-right: 30rpx;
				}

				.original_price {
					color: #ccc;
					font-size: 16px;
					text-decoration: line-through;
				}
			}

			.title {
				padding: 10rpx 20rpx;
				font-size: 32rpx;
				margin: 16rpx 0;
			}

			.goods_msg {
				border-top: 8rpx solid #eee;
				border-bottom: 8rpx solid #eee;
				font-size: 32rpx;

				.number {
					padding: 10rpx 20rpx;
				}

				.stock_quantity {
					padding: 10rpx 20rpx;
				}
			}
		}

		.introduced {
			padding: 10rpx 20rpx;
			font-size: 32rpx;
			margin-bottom: 50px;

			.indd_title {
				font-size: 36rpx;
				height: 80rpx;
				line-height: 80rpx;
				margin-bottom: 20rpx;
				border-bottom: 1px solid #eee;
			}

			.indd_content {
				line-height: 48rpx;
				width: 355px;

				.gomeImgLoad {
					width: 355px;
				}
			}
		}
	}
	
	.goods_nav {
		position: fixed;
		left: 0;
		right: 0;
		bottom: 0;
	}
</style>
