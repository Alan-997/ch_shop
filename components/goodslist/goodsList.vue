<template>
	<view class="goods-list">
		<goods-item 
				v-for="(item, index) in goods" :key="index" 
				:goodsItem="item" >
		</goods-item>
	</view>
</template>

<script>
	import goodsItem from './goodsItem.vue'
	export default {
		data() {
			return {
				
			};
		},
		props: {
			goods: {
				type: Array,
				default () {
					return []
				}
			}
		},
		components: {
			goodsItem
		}
	}
</script>

<style>
	.goods-list {
		padding: 0 15rpx;
		display: flex;
		justify-content: space-between;
		flex-wrap: wrap;
	}
</style>
