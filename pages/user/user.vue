<template>
	<view class="user">
		<view class="records">
			<image src="../../static/images/history.png" mode="aspectFill"></image>
			<text>浏览历史</text>
		</view>
		<view class="content">
			<view class="row" v-for="(item,index) in listArr" :key="index">
				<newsItem :item='item' @click.native="goDetail(item)"></newsItem>
			</view>
		</view>

		<view class="nohistory" v-if="!listArr.length">
			<image src="../../../uniapp/uniapp-ling_project/青年帮新闻_课堂版/static/images/nohis.png" mode="widthFix"></image>
			<view class="text">
				暂无浏览记录
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listArr: [],
			};
		},
		onShow() {
			this.getData()
		},
		methods: {
			goDetail(item) {
				uni.navigateTo({
					url: `/pages/details/details?cid=${item.classid}&id=${item.id}`
				})
			},
			// 获取缓存浏览记录
			getData() {
				let hisArr = uni.getStorageSync("historyArr") || []
				this.listArr = hisArr
				console.log(this.listArr);
			}
		}
	}
</script>

<style lang="scss" scoped>
	.user {
		.records {
			padding: 20rpx 0 0;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;

			image {
				width: 100rpx;
				height: 100rpx;
				margin-bottom: 10rpx;
			}

			text {
				color: #31c27c;
			}
		}

		.content {
			padding: 30rpx;

			.row {
				border-bottom: 1px dotted #efefef;
				padding: 20rpx 0;
			}
		}

		.nohistory {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			image {
				width: 450rpx;
			}

			.text {
				font-size: 26rpx;
				color: #888;
			}
		}
	}
</style>
