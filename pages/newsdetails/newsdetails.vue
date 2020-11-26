<template>
	<view class="news_details">
		<view class="details"  v-for="(item,index) in newsDetails" :key="index">
			<view class="details_title">
				<view class="title">{{item.title}}</view>
				<view class="info">
					<text>发表时间：{{item.add_time | formatDate}}</text>
					<text>浏览：{{item.click}}</text>
				</view>
			</view>
			<view class="details_content" v-html="item.content"></view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: 0,
				newsDetails: []
			}
		},
		filters: {
			formatDate (date) {
				const nDate = new Date(date)
				const year = nDate.getFullYear()
				const month = nDate.getMonth().toString().padStart(2,0)
				const day = nDate.getDay().toString().padStart(2,0)
				return year+'-'+month+'-'+day
			}
		},
		methods: {
			getNewsDetails () {
				uni.request({
					url:"http://localhost:8082/api/getnew/" + this.id,
					success:res => {
						// console.log(res)
						if(res.data.status !== 0){
							return uni.showToast({
								title: "请求失败！"
							})
						}
						this.newsDetails = res.data.message
					}
				})
			}
		},
		onLoad(options) {
			this.id = options.id	// 这个要写前面？
			this.getNewsDetails ()
		}
	}
</script>

<style lang="scss">
.details {
	padding: 10rpx 20rpx;
	.details_title {
		width: 750rpx;
		.title {
			font-size: 40rpx;
			width: 710rpx;
		}
		.info {
			font-size: 30rpx;
			text {
				margin-right: 60rpx;
			}
		}
	}
	.details_content {
		font-size: 32rpx;
		margin-top: 30rpx;
	}
}
</style>
