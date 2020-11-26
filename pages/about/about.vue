<template>
	<view class="about">
		<view class="about_img">
			<image class="img" src="http://www.itcast.cn/2018czydz/images/gywmban.jpg" mode=""></image>
		</view>
		<view class="about_info">
			<view class="number" @click="phoneClick">
				联系电话：18300871586 
			</view>
			<view class="address">
				地址：贵阳市云岩区轮胎厂西山巷星马商住楼602
			</view>
		</view>
		<map :latitude="latitude" :longitude="longitude" :markers="markers"></map>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				latitude: 26.585766,
				longitude: 106.651434,
				scale: 10,
				markers: [
					{
						latitude: 26.585766,
						longitude: 106.651434,
						iconPath: '../../static/tx.jpg',
						width: 40,
						height:40
					}
				]
			}
		},
		methods: {
			phoneClick () {
				uni.makePhoneCall({
					phoneNumber: "18300871586"
				})
			}
		}
	}
</script>

<style lang="scss">
.about_img {
	.img {
		width: 750rpx;
		height: 320rpx;
	}
}

.about_info {
	font-size: 30rpx;
	line-height: 60rpx;
	view {
		padding: 30rpx 20rpx;
		border-bottom: 1px solid #eee;
	}
}
map {
	width: 750rpx;
	height: 750rpx;
}
</style>
