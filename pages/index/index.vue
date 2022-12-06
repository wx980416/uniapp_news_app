<template>
	<view class="home">
		<scroll-view scroll-x="true" class="nav-scroll">
			<view v-for="(item,index) in navArr" :key="item.id" class="item" :class="{active:navIndex === index}"
				@click="clickNav(index,item.id)">{{item.classname}}</view>
		</scroll-view>

		<view class="content">
			<view class="row" v-for="(item,index) in newsArr" :key="item.id">
				<newsItem :item='item' @click.native='getDetail(item)'></newsItem>
			</view>
		</view>

		<view class="nodata" v-if="!newsArr.length">
			<image src="../../static/images/nodata.png" mode="widthFix"></image>
		</view>

		<view class="loading" v-if="newsArr.length">
			<view v-if="loading === 1">
				数据加载中
			</view>
			<view v-if="loading === 2">没有更多了</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				navIndex: 0,
				navArr: [],
				newsArr: [],
				currentPage: 1,
				loading: 0 // 0 默认 1 加载中 2 没有更多了
			}
		},
		onLoad() {
			this.getNavData()
			this.getNewsData()
		},
		onReachBottom() {
			console.log('滚动到底部了');
			if (this.loading === 2) {
				return
			}
			this.currentPage++
			this.loading = 1
			this.getNewsData()
		},
		methods: {
			// 获取新闻信息列表
			getNewsData(id = 50) {
				uni.request({
					url: 'https://ku.qingnian8.com/dataApi/news/newslist.php',
					data: {
						cid: id,
						page: this.currentPage
					},
					success: res => {
						console.log(res);
						if (res.data.length === 0) {
							this.loading = 2
						}
						this.newsArr = [...this.newsArr, ...res.data]
					}
				})
			},
			// 获取导航列表数据
			getNavData() {
				uni.request({
					url: 'https://ku.qingnian8.com/dataApi/news/navlist.php',
					success: res => {
						console.log(res);
						this.navArr = res.data
					}
				})
			},
			// 跳转倒详情页
			getDetail(item) {
				uni.navigateTo({
					url: `/pages/details/details?cid=${item.classid}&id=${item.id}`
				})
			},
			// 点击导航切换
			clickNav(index,id) {
				this.navIndex = index;
				this.currentPage = 1;
				this.newsArr = [];
				this.loading = 0;
				this.getNewsData(id);
			},
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

		.nodata {
			display: flex;
			justify-content: center;

			image {
				width: 360rpx;
			}
		}

		.loading {
			text-align: center;
			font-size: 26rpx;
			color: #888;
			line-height: 2em;
		}
	}
</style>
