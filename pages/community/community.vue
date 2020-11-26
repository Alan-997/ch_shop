<template>
	<view class="community">
		<scroll-view scroll-y="true" class="left">
			<view :class = "(active === index) ? 'active' : ''" 
						v-for="(item, index) in leftList"
						:key="index"
						@click="listClickHandle(index, item.id)"
						>
						{{item.title}}</view>
		</scroll-view>
		
		<scroll-view scroll-y="true" class="right">
			<view v-for="(item, index) in rightLits" :key="index">
				<image :src="item.img_url" mode="" @click="imageShowClick(item.img_url)"></image>
				<text>{{item.title}}</text>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				leftList: [],
				rightLits: [],
				active: 0,
				rImgUrl: []
			}
		},
		methods: {
			getCommunityLeft () {
				uni.request({
					url:'http://localhost:8082/api/getimgcategory',
					success: res => {
						// console.log(res)
						if(res.data.status !== 0){
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.leftList = res.data.message
					}
				})
			},
			listClickHandle (index, id) {
				// console.log(id)
				this.active = index
				uni.request({
					url:'http://localhost:8082/api/getimages/' + id,
					success: res => {
						console.log(res)
						if(res.data.status !== 0){
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.rightLits = res.data.message
						for(let i = 0; i <= this.rightLits.length - 1; i++){
							this.rImgUrl.push(this.rightLits[i].img_url)
						}
					}
				})
			},
			imageShowClick (current) {
				uni.previewImage({
					current,
					urls:this.rImgUrl,
					loop: true,
					indicator: Number
				})
			}
		},
		onLoad() {
			this.getCommunityLeft ()
		}
	}
</script>

<style lang="scss">
page {
	height: 100%;
	.community {
		height: 100%;
		display: flex;
		.left {
			width: 200rpx;
			height: 100%;
			font-size: 30rpx;
			border-right: 1px solid #eee;
			view {
				height: 120rpx;
				line-height: 120rpx;
				border-top: 1px solid #eee;
				text-align: center;
			}
			.active {
				background-color: #B50E03;
				color: #fff;
			}
		}
		.right {
			width: 520rpx;
			height: 100%;
			margin: 10rpx auto;
			font-size: 30rpx;
			view {
				margin: 15rpx 0;
				image {
					width: 100%;
					border-radius: 20rpx;
				}
			}
		}
	}
}
</style>
