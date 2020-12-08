<template>
	<view>
		<uni-list-item title="修改密码" @click="open('modify-password')"></uni-list-item>
		<uni-list-item title="绑定手机"></uni-list-item>
		<uni-list-item title="绑定微信"></uni-list-item>
		<uni-list-item title="绑定QQ"></uni-list-item>
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
		//封装根据path跳转页面的方法
		open(path) {
			uni.navigateTo({
				url: `../${path}/${path}`
			});
		},
	}
};
</script>

<style></style>
