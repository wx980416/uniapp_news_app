<template>
	<view class="home">
		<scroll-view scroll-x="true" class="nav-scroll">
			<view v-for="(item,index) in 10" :key="index" class="item" :class="{active:activeIndex === index}"
				@click="handleClickNav(index)">国内</view>
		</scroll-view>

		<view class="content">
			<view class="row" v-for="(item,index) in 20" :key="index">
				<newsItem></newsItem>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				activeIndex: 0,
			}
		},
		onLoad() {

		},
		methods: {
			handleClickNav(index) {
				console.log(index);
				this.activeIndex = index;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.home {
		.nav-scroll {
			height: 100rpx;
			white-space: nowrap;
			background: #F7F8FA;

			.item {
				font-size: 40rpx;
				display: inline-block;
				line-height: 100rpx;
				padding: 0 30rpx;
				color: #333;

				&.active {
					color: #31c27c;
				}
			}


			// 隐藏 h5 端滚动条
			/deep/ ::-webkit-scrollbar {
				width: 4px !important;
				height: 1px !important;
				overflow: auto !important;
				background: transparent !important;
				-webkit-appearance: auto !important;
				display: block;
			}
		}

		.content {
			padding: 30rpx;

			.row {
				border-bottom: 1px dotted #efefef;
				padding: 20rpx 0;
			}
		}
	}
</style>
