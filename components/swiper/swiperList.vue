<template>
	<view class="swiper">
		<swiper class="swiper" indicator-dots circular autoplay>
			<swiper-item class="swiper-item" v-for="(item, index) in swipers" :key="index">
				<image :src="item.img"></image>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data () {
			return {}
		},
		props:{
			swipers: {
				type: Array,
				default () {
					return []
				}
			}
		}
	}
</script>

<style lang="scss">
	.swiper {
		width: 750rpx;
		height: 380rpx;

		.swiper-item {
			image {
				width: 100%;
				height: 100%;
			}
		}
	}
</style>
