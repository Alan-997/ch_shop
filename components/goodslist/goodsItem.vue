<template>
	<view class="goods-item" @click="goodsItemClick(goodsItem.id)">
		<image :src="goodsItem.img_url" mode=""></image>
		<view class="price">
			<text class="present-price">￥{{goodsItem.sell_price}}</text>
			<text class="original-price">￥{{goodsItem.market_price}}</text>
		</view>
		<view class="goods-info">
			<text class="goods-name">{{goodsItem.title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data () {
			return {
				
			}
		},
		props:{
			goodsItem: {
				type: Object,
				default () {
					return {}
				}
			}
		},
		methods: {
			goodsItemClick (id) {
				uni.navigateTo({
					url: '../../pages/details/details?id=' + id
				})
			}
		}
	}
</script>

<style lang="scss">
	.goods-item {
		width: 355rpx;
		background-color: #fff;
		margin: 10rpx 0;
		padding: 15rpx;
		box-sizing: border-box;
		border-radius: 10rpx;
		image {
			width: 80%;
			height: 150px;
			display: block;
			margin: 0 auto;
			text-align: center;
		}
		.price {
			.present-price {
				color: #CF635C;
				font-size: 18px;
				margin-right: 30rpx;
			}
			.original-price {
				color: #ccc;
				font-size: 16px;
				text-decoration: line-through;
			}
		}
		.goods-info {
			font-size: 14px;
			margin-top: 12rpx;
		}
	}
</style>
