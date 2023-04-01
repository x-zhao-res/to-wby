<template>
    <view class="border">
        <map style="width: 100%;height: 400px;" :latitude="mapStorage.latitude" :longitude="mapStorage.longitude"></map>
        <!-- https://uniapp.dcloud.net.cn/component/map.html# -->
        <!-- 此方案限制对地图交互限制极大，已经废弃 -->
    </view>
</template>

<script>
	export default {
		data() {
			return {
				title: '12314421233123',
                mapStorage:{
                    id:0,
                    title:'map',
                    latitude: 39.909,//横坐标
			        longitude: 116.39742,//纵坐标
                    //要注意，获取位置的API和地图组件不是一个东西，需要分开请求
                }
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style scoped>
    .border{
        width: 100%;
        height: 100%;
        border: 1px solid black;
    }
</style>
