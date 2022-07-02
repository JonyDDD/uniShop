<template>
	<view class="floor-list">
		<!-- 楼层 item 项 -->
		<view class="floor-item" v-for="(item, i) in floorList" :key="i">
			<!-- 楼层标题 -->
			<image :src="item.floor_title.image_src" class="floor-title"></image>
			<!-- 楼层图片-->
			<view class="floor-img-box">
				<!-- 左侧 -->
				<navigator :url="item.product_list[0].toUrl" class="left-img-box">
					<img :src="item.product_list[0].image_src" :style="{width:item.product_list[0].image_width+'rpx'}"
						mode=widthFix>
				</navigator>
				<!-- 右侧 -->
				<view class="right-img-box">
					<navigator class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2 !== 0" :url="item2.toUrl">
						<image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}">
						</image>
					</navigator>
				</view>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {

			};
		},
		props: ['floorList'],
		onLoad() {
			console.log(this.floorList);
		}
	}
</script>

<style lang="scss">
	.floor-title {
		height: 60rpx;
		width: 100%;
		display: flex;
	}
	.floor-img-box{
		display: flex;
	}
	.right-img-box{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}
</style>
