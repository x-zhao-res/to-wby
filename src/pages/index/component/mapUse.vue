<template>
    <view class="border">
        <view 
		:market="markerList" 
		id="amap" 
		style="width: 100%;height: 400px;"
		:change:market="amap.mapRecive" ></view>
		<view @click="test()">{{ this.renderData.location[0] }}</view>
		<view>{{ this.renderData.location[1] }}</view>
    </view>
</template>
<script>
	export default {
		data() {
			return {
				title: '12314421233123',
                markerList: [
					[116.352258,39.916811],
					[116.344469,39.934095]
				],
				renderData:{}
			}
		},
        mounted(){
			this.test
        },
		onLoad() {
		},
		methods: {
		test(){
			// this.markerList.push([116.527076,39.881388]) //此代码测试数据传参
		},
        getMapData() {
			this.markerList = [
				{
					lat: 39.908775,
					lng: 116.406315,
					icon: start
				},
				{
					lat: 39.973253,
					lng: 116.473195,
					icon: start
				},
				{
					lat: 39.953253,
					lng: 116.453195,
					icon: start
				}
			]
		},
        onViewClick(params) {
			this.dataIndex = params.dataIndex
		},
		reviceRender(data){
			this.renderData = {
				...this.renderData,
				...data
			}
		}
		}
	}
</script>
<script module="amap" lang="renderjs">
 export default {
    data(){
        return{
            map:null,
        }
    },
    mounted() {
        if (typeof window.AMap === 'function') {
			this.initAmap()
		} else {
			// 动态引入较大类库避免影响页面展示
			//这部分直接复制就行
			const script = document.createElement('script')
			script.src = 'https://webapi.amap.com/maps?v=2.0&key=' + '9939262d4403feda5891e99066216ddc'
			script.onload = this.initAmap.bind(this)
			document.head.appendChild(script)
		}
		console.log(this.markerList)
    },
    methods: {
        initAmap() {
			this.map = new AMap.Map('amap', {
				resizeEnable: true,
				center: [116.397428, 39.90923],
				viewMode: '2D',
				zooms: [4, 18], //设置地图级别范围
				zoom: 10
			})
			// this.markerInit() //初始化标点
			this.mapListen() //监听事件挂载
			this.youLocation()
		},
		markerInit(){			
			var market = null
			for (let marketIndex = 0; marketIndex < this.markerList.length; marketIndex++) {
				market = new AMap.Marker({
            		icon: "http://a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-default.png",
					position: this.markerList[marketIndex],
            		offset: new AMap.Pixel(-13, -30),
					extData:{id:'asdwkdjf'}
        		});
				market.on('click',this.mapMarketClick) //挂载点标记点击事件
				this.map.add(market) //挂载覆盖物到地图
			}
		    // this.map.setFitView(null,true,[60,60,60,60])
			// 第一个参数为空，表明用图上所有覆盖物 setFitview
        	// 第二个参数为false, 非立即执行
        	// 第三个参数设置上左下右的空白
		},
		mapListen(){
			//地图点击事件
			this.map.on('click',this.clickMap)
		},
		clickMap(e){
			//点击事件的回调,下面是数组，更多的去高德官网找
			this.mapMoveChange(e.lnglat)
		},
		mapMoveChange(locationArray){
			// 移动地图
			this.map.panTo(locationArray);
			this.dataTransform({ location:locationArray }) //向vue传输坐标
		},
		mapMarketClick(e){
			console.log(e) //点坐标点击后输出
		},
		dataTransform(data){
			//这里是从render向vue基础逻辑层传数据,我设置了一个需要传输上去的大对象，传过去的都会被扔到那个里面
			this.$ownerInstance.callMethod('reviceRender', data)
		},
		mapRecive(newValue,oldValue,ownerInstance,instance){
			console.log(newValue)
		},
		youLocation(){
			//开启定位
			this.map.plugin('AMap.Geolocation', () => {
				var geolocation = null;
				geolocation = new AMap.Geolocation({
					enableHighAccuracy: true,//是否使用高精度定位，默认:true
					timeout: 10000,          //超过10秒后停止定位，默认：无穷大
					maximumAge: 10,           //定位结果缓存0毫秒，默认：0
					convert: false,           //自动偏移坐标，偏移后的坐标为高德坐标，默认：true
					showButton: false,        //显示定位按钮，默认：true
					// buttonPosition: 'LB',    //定位按钮停靠位置，默认：'LB'，左下角
					// buttonOffset: new AMap.Pixel(10, 20),//定位按钮与设置的停靠位置的偏移量，默认：Pixel(10, 20)
					showMarker: true,        //定位成功后在定位到的位置显示点标记，默认：true
					showCircle: false,        //定位成功后用圆圈表示定位精度范围，默认：true
					panToLocation: true,     //定位成功后将定位到的位置作为地图中心点，默认：true
					zoomToAccuracy:true      //定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
				});
				this.map.addControl(geolocation);
				geolocation.getCurrentPosition();
				this.map.setZoom(16)
				//下面这个是返回信息，我觉得可以不用要,但是如果你想自己打点，那就需要
				// AMap.event.addListener(geolocation, 'complete', onComplete);//返回定位信息
				// AMap.event.addListener(geolocation, 'error', onError);      //返回定位出错信息
			});
		}
    },
 }
</script>

<style scoped>
    .border{
        width: 100%;
        height: 100%;
        border: 1px solid black;
    }
</style>
