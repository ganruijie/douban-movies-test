<template>
	<div class="md-splash">
		<swiper class="splash" indicator-dots='true'>
			<block v-for="(item,index) in movies" :key="index">
			    <swiper-item>
			      <image :src="item.images.large" class="slide-image" mode="aspectFill"/>
			      <button class="start" @click="handleStart" v-if="index == movies.length - 1">立即体验</button>
			    </swiper-item>
			</block>
		</swiper>
	</div>
</template>

<script>
	import {getStorage,setStorage} from '../../utils/wechat'
	import {getBoardData} from '../../utils/api'
	const LAST_SPLASH_DATA = 'LAST_SPLASH_DATA' 
	export default{
		data(){
			return{
				movies:[]
			}
		},
		methods:{
			async getCache(){
				try{
					let res = await getStorage(LAST_SPLASH_DATA)
					const {movies,expires} = res.data
					
					if(movies && expires > Date.now()){
						return res.data
					}
					//一天的过期时间
					console.log('uncached')
					return null;
				}catch(error){
					return null
				}
			},
			handleStart(){
				wx.switchTab({
					url:'../board/main'
				})
			},
			async getInitData(){
				let cache = await this.getCache()
				if(cache){
					this.movies = cache.movies
					return
				}
				let data = await getBoardData({board:'coming_soon',page:1,count:3})
				
				this.movies = data.subjects
				console.log(this.movies,'this.movies')
				await setStorage(LAST_SPLASH_DATA,{
					movies:data.subjects,
					expires:Data.now() + 1*24*60*60*1000
				})
			}
		},
		mounted(){
			this.getInitData()
		}
	}
</script>

<style>
	page {
  height: 100%;
}

.container {
  height: 100%;
}

.md-splash {
  height: 100%;
}

.splash {
  height: 100%;
}

.splash swiper-item {
  flex: 1;
}

.splash swiper-item image {
  position: absolute;
  height: 100%;
  width: 100%;
  opacity: .9;
}

.start {
  position: absolute;
  bottom: 200rpx;
  left: 50%;
  width: 300rpx;
  margin-left: -150rpx;
  background-color: rgba(64, 88, 109, .4);
  color: #fff;
  border: 1rpx solid rgba(64, 88, 109, .8);
  border-radius: 200rpx;
  font-size: 40rpx;
}

</style>