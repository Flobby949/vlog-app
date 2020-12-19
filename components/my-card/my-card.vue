<template>
	<view>
		<view class="my-card" @click="open">
			<view class="thumb">
				<image :src="article.cover" class="img" mode="aspectFill" />
				<view class="category">{{ article.category }}</view>
			</view>
			<view class="detail">
				<view class="mid">
					<view class="title">{{ article.title }}</view>
					<view class="summary">{{ article.summary.length > 30 ? article.summary.substring(0, 30) : article.summary }}</view>
				</view>
				<view class="right"></view>
			</view>
		</view>
		<view class="other">
			<view class="date">{{ article.publishDate }}</view>
			<view class="tags">
				<button class="tag primary" v-for="(tag, index) in article.tagList" :key="index">{{ tag.tagName }}</button>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	name: 'MyCard',
	props: ['article'], //通过props传值，父组件中将article传过来
	computed: {
		//计算属性，文章摘要的长度超出，就用...代替
		summary() {
			let summary = this.article.summary;
			if (summary && summary.length > 30) {
				summary = summary.substring(0, 30) + '...';
			}
			return summary;
		}
	},
	methods: {
		open() {
			console.log('click');
			//在子组件mycard。vue中将点击事件传回父组件index.vue的@open
			//emit中写的就是方法名
			this.$emit('open');
		}
	}
};
</script>

<style lang="scss" scoped>
.my-card {
	padding: 20rpx 20rpx;
	border-bottom: 1px dashed #ddd;
	display: flex;
	background-color: #ffffff;
	font-size: 14px;
	.thumb {
		width: 200rpx;
		height: 220rpx;
		position: relative;
		.img {
			width: 150rpx;
			height: 150rpx;
			border-radius: 10%;
			position: absolute;
			top: 10rpx;
			left: 10rpx;
		}
		.category {
			color: #754a8f;
			padding: 2px 2px;
			position: absolute;
			bottom: 0;
			left: 10rpx;
		}
	}
	.detail {
		padding-left: 20px;
		word-wrap: break-word;
		width: 550rpx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		.mid {
			.title {
				font-weight: bold;
			}
		}
		.right {
			width: 70px;
			text-align: right;
		}
	}
}
.other {
	display: flex;
	flex-direction: column;
	padding-bottom: 5rpx;
	border-bottom: 1px solid #ddd;
	.date {
		margin-left: 30rpx;
		font-weight: bold;
	}
	.tags {
		display: flex;
		flex-direction: row;
		.tag {
			position: relative;
			margin-left: 8rpx;
			margin-right: 8rpx;
			font-size: 14px;
			height: 60rpx;
			width: 100px;
		}
	}
}
</style>
