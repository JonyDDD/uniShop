<template>
	<view>
		<view class="nav-list">
			<view class="nav-item" v-for="(item, i) in navList" :key="i" @click="navChange(item)">
				<image :src="item.image_src" class="nav-img"></image>
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	export default {
		name: "CateList",
		data() {
			return {

			};
		},
		onLoad() {

		},
		methods:{
			navChange(item){
				if(item.name ==='分类') return uni.switchTab({ url : '/pages/cate/cate'})
			}
		},
		props: ['navList']
	}
</script>

<style lang="scss">
.nav-list{
	display: flex;
	justify-content: space-around;
	margin: 15px 0 ;
	.nav-img{
		width: 128rpx;
		height: 140rpx;
	}
}
</style>
