<template>
	<view>
		<!-- 轮播图 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item,index) in swiperList" :key='index'>
				<navigator  :url='`/subpkg/goodsDetail/goodsDetail?goods_id=${item.goods_id}`'   class="swiper-item" >
					<image :src="item.image_src" alt="" />
				</navigator>
			</swiper-item>
		</swiper>
		<!-- 分类组件 -->
		<CateList :navList="navList" v-if="navList"/> 
		<!-- 楼层组件 -->
		<Floor :floorList="floorList" v-if="floorList"/>
		
	</view>
</template>

<script>
	import CateList  from "./CateList.vue"
	import Floor  from "./Floor.vue"
	export default {
		data() {
			return {
				swiperList:[],
				navList:[],
				floorList:[]
			};
		},
		components:{
			CateList,
			Floor
		},
		onLoad() {
			this.getSwiperInfo()
			this.getCateInfo()
			this.getFloorInfo()
		},
		methods:{
			async getSwiperInfo(){
				const res = await uni.$http.get('/home/swiperdata');
				if(res.data.meta.status !== 200) return uni.$showMsg()
				this.swiperList = res.data.message
			},
			async getCateInfo(){
				const res = await uni.$http.get('/home/catitems');
				if(res.data.meta.status !== 200) return uni.$showMsg()
				this.navList = res.data.message
			},
			async getFloorInfo(){
				const res = await uni.$http.get('/home/floordata');
				if(res.data.meta.status !== 200) return uni.$showMsg()
				res.data.message.forEach(i1 =>{
					i1.product_list.forEach(i2=>{
						i2.toUrl = '/subpkg/goodsDetail/goodsDetail?' + i2.navigator_url.split('?')[1]
					})
				})
				this.floorList = res.data.message
			}
		}
	
	}
</script>

<style lang="scss">
swiper{
	height: 330rpx;
	.swiper-item,
	image{
			width: 100%;
	}
}


</style>
