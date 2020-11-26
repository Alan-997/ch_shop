<template>
	<view class="goods">
		<goods-list :goods="goods"></goods-list>
		<view class="bottom_line" v-if="flag">-----我是有底线的-----</view>
	</view>
</template>

<script>
	import goodsList from "../../components/goodslist/goodsList.vue"
	export default {
		data() {
			return {
				goods: [],
				pageindex: 1,
				flag: false,
				detailsPath: '../../pages/details/details'
			}
		},
		components:{
			goodsList
		},
		onLoad() {
			this.getGoods()
		},
		methods: {
			getGoods(callBack) {
				uni.request({
					url:"http://localhost:8082/api/getgoods?pageindex="+this.pageindex,
					success: res => {
						// console.log(res)
						if(res.data.status !== 0){
							return uni.showToast({
								title: "请求失败！"
							})
						}
						// 数组内的this.goods指的是之前请求的数据，res.data.message是指当前请求的数据
						// 将两个都展开存在数组中展示，
						// this,goods = res.data.mesage 如果以这样的方法请求，当前的数据会覆盖之前的数据
						this.goods = [...this.goods, ...res.data.message]
						
						// 当数据请求完成之后关闭刷新的图标
						// uni.stopPullDownRefresh()
						// 但是注意逻辑，不能在每一次数据请求完成的时候去调用关闭函数，因为数据的请求完成有上拉加载，下拉刷新等，所以这里的关闭刷新函数需要在刷新结束的时候调用
						// callBack() 注意，这里传入的callBack也不能直接调用，因为我们的数据请求是在页面加载的时候就已经请求了，此时还找不到callBack，所以需要判断callBack是否存在
						callBack && callBack()
					}
				})
			},
			
		},
		// 上拉加载更多
		onReachBottom () {
			// console.log('到底了！')
			// 这里必须做一个判断
			// 判断数据是否还有，如果数据没有了，再次上拉就不再请求，并且显示底部的提示
			// 这里的判断是不太正确的，以后判断再注意方法
			if(this.goods.length<this.pageindex * 10) return this.flag = true
			this.pageindex++
			this.getGoods()
		},
		// 下拉刷新
		onPullDownRefresh () {
			this.pageindex = 1
			this.flag = false
			this.goods = []
			setTimeout(()=>{
				this.getGoods(() => {
					uni.stopPullDownRefresh()
				})
			},1000)
		}
	}
</script>

<style lang="scss">
	.goods {
		background-color: #eee;
		padding-top: 10rpx;
		.bottom_line {
			font-size: 28rpx;
			height: 100rpx;
			line-height: 100rpx;
			color: #929292;
			text-align: center;
		}
	}
</style>
