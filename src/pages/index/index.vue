<template>
	<view class="bodyuse">
		<map-use></map-use>
		<!-- <mapuseuni></mapuseuni>	 -->
	</view>
</template>

<script>
import mapUse from './component/mapUse.vue';
import mapuseuni from './component/mapuseuni.vue';
	export default {
		data() {
			return {
				title: '12314421233123'
			}
		},
		onLoad() {

		},
		components:{
			mapUse,
			mapuseuni
		},
		methods: {

		}
	}
</script>

<style scoped>
	.bodyuse{
		width: 100%;
		height: 700px;
		/* 这里是为了保证父元素足够长 */
	}
</style>
