<template>
	<view>
		<view class="text-center">
			<image src="../../static/common/nothing.png" class="center mb-2" style="width: 250rpx;height: 250rpx;border-radius: 80rpx;"></image>
			<view style="color: #999999; font-size: 30rpx;">version 1.0.0</view>
		</view>
		<uni-list-item title="新版本检测"></uni-list-item>
		<uni-list-item title="社区用户协议"></uni-list-item>
	</view>
</template>

<script>
import uniListItem from '@/components/uni-ui/uni-list-item/uni-list-item.vue';
export default {
	components: {
		uniListItem
	},
	data() {
		return {
			currentSize: 0
		};
	},
	onLoad() {
		
	},
	methods: {
		
	}
}
</script>

<style>

</style>
