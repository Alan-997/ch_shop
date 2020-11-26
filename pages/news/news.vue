<template>
	<view class="news">
		<view class="news_list">
			<view class="news_item" 
						v-for="item in newsList" 
						@click="newsDetails(item.id)">
				<image :src="item.img_url" mode=""></image>
				<view class="item_info">
					<view class="title">{{item.title}}</view>
					<view class="info">
						<text>发表时间：{{item.add_time | formatDate}}</text>
						<text>浏览：{{item.click}}</text>
					</view>
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				newsList: []
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
			getNewsHandle () {
				uni.request({
					url:"http://localhost:8082/api/getnewslist",
					success: res=> {
						console.log(res)
						if(res.data.status !== 0){
							return uni.showToast({
								title:"请求失败！"
							})
						}
						this.newsList = res.data.message
					}
				})
			},
			newsDetails (id) {
				// console.log(id)
				uni.navigateTo({
					url:"../newsdetails/newsdetails?id="+ id
				})
			}
		},
		onLoad() {
			this.getNewsHandle ()
		}
	}
</script>

<style lang="scss">
	.news_list {
		.news_item {
			display: flex;
			width: 750rpx;
			height: 172rpx;
			padding: 10rpx 20rpx;
			border-bottom: 1px solid #B50E03;
			image {
				width: 200rpx;
				height: 150rpx;
			}
			.item_info {
				margin-left: 30rpx;
				display: flex;
				flex-wrap: wrap;
				align-content: space-around;
				width: 450rpx;
				.title {
					font-size: 30rpx;
				}
				.info {
					font-size: 24rpx;
					text{
						margin-right: 20rpx;
					}
				}
			}
		}
	}
</style>
