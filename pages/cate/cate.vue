<template>
	<view>
		<view class="scroll-view-container">
			<!-- 左侧滚动 -->
			<scroll-view scroll-y="true" class="scroll-view-left" :style="{height: wh + 'px'}">
				<view :class="['left-scroll-view-item', i === activeIndex ? 'active' : '']" v-for="(item,i) in cateList"
					:key="item.cat_id" @click="changeActive(i)">{{item.cat_name}}</view>
			</scroll-view>
			<!-- 右侧滚动 -->
			<scroll-view scroll-y="true" class="scroll-view-right" :style="{height: wh + 'px'}" :scroll-top="scrollTop">
				<!-- 二级标题 -->
				<view class="cate-lv2" v-for="(item2,i2) in cateLevel2" :key="i2">
					<view class="cate-lv2-title"> ---- {{item2.cat_name}} ----</view>
					<!-- 三级内容 -->
					<view class="cate-lv3">
						<view class="cate-lv3-item" v-for="(item3,i3) in item2.children" :key="i3" @click="toGoodsDetail(item3)">
							<img :src="item3.cat_icon" alt="">
							<text>{{item3.cat_name}}
						</view>
					</view>
				</view>
		</view>
		</scroll-view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				wh: 0,
				cateList: [],
				cateLevel2: [],
				activeIndex: 0,
				// 滚动条距离顶部的距离
				scrollTop: 0
			}
		},
		onLoad() {
			const sysInfo = uni.getSystemInfoSync()
			console.log(sysInfo);
			this.wh = sysInfo.windowHeight
			this.getCateInfo()
		},
		methods: {
			async getCateInfo() {
				const {
					data: res
				} = await uni.$http.get('/categories')
				if (res.meta.status !== 200) return uni.$showMsg()
				this.cateList = res.message
				this.cateLevel2 = res.message[0].children

			},
			changeActive(index) {
				this.activeIndex = index
				uni.showLoading({
					title: '数据加载中',
					duration: 300
				})
				this.cateLevel2 = this.cateList[index].children
				this.scrollTop = this.scrollTop ? 0 : 1
			},
			toGoodsDetail(item){
				uni.navigateTo({
					url:'/subpkg/goodsList/goodsList?id=' + item.cat_id
				})
			}
		}
	}
</script>

<style lang="scss">
	.scroll-view-container {
		display: flex;

		.scroll-view-left {
			width: 120px;

			.left-scroll-view-item {
				line-height: 60px;
				background-color: #f7f7f7;
				text-align: center;
				font-size: 12px;

				&.active {
					background-color: #ffffff;
					position: relative;

					&::before {
						content: ' ';
						display: block;
						width: 3px;
						height: 30px;
						background-color: #c00000;
						position: absolute;
						left: 0;
						top: 50%;
						transform: translateY(-50%);
					}
				}
			}
		}

		.cate-lv2-title {
			font-size: 12px;
			font-weight: bold;
			text-align: center;
			padding: 15px 0;
		}

		.cate-lv3 {
			display: flex;
			flex-wrap: wrap;

			.cate-lv3-item {
				width: 33.33%;
				display: flex;
				flex-direction: column;
				align-items: center;
				margin-bottom: 10px;

				img {
					width: 60px;
					height: 60px;
				}

				text {
					font-size: 12px;
				}
			}
		}
	}
</style>
