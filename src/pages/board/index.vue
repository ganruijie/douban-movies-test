<template>
	<div class="board-box">
		<view class="board-box-top">
			<swiper :indicator-dots="true" :autoplay="true" :interval="2000" :duration="1000">
			  <block v-for="(item,index) in movies" :key="index">
			    <swiper-item>
			      <image :src="item.images.medium" class="slide-image"/>
			    </swiper-item>
			  </block>
			</swiper>
		</view>
		<view class="board-box-bottom board" :scroll-y="true">
			<block v-for="(item,index) in boards" :key="index">
				<view class="board-item">
					<navigator :url="'../list/main?type='+item.key+'&title='+item.title" hover-class="none">
						<view class="title">
							<text class="text">{{item.title}}</text>
							<image src="../../../static/images/arrowright.png" mode="aspectFill"/>
						</view>
					</navigator>
					<scroll-view class="content" :scroll-x="true">
					    <view v-if="item.key !== 'us_box'" class="inner">
					    	<block v-for="(movie,ind) in item.movies" :key="ind">
					    		<navigator :url="'../item/main?id=' + movie.id">
					    			<view class="movie-item">
					    				<image :src="movie.images.large" mode="aspectFill"></image>
					    				<text>{{movie.title}}</text>
					    			</view>
					    		</navigator>
					    	</block>
					    </view>
					    <view v-else class="inner">
					    	<block v-for="(movie,ind) in item.movies" :key="ind">
					    		<navigator :url="'../item/main?id=' + movie.id">
					    			<view class="movie-item">
					    				<image :src="movie.subject.images.large" mode="aspectFill"></image>
					    				<text>{{movie.subject.title}}</text>
					    			</view>
					    		</navigator>
					    	</block>
					    </view>
					</scroll-view>
				</view>
			</block>
		</view>
	</div>
</template>

<script>
	import { mapState , mapActions } from 'vuex'
	export default{
		data(){
			return{
				
			}
		},
		computed:{
			...mapState('board',{
				boards:state=>state.boards,
				movies:state=>state.movies
			})
		},
		methods:{
			...mapActions('board',[
				'getBoards'
			]),
			async getBoardData(){
				this.getBoards()
			}
		},
		mounted(){
			this.getBoardData();
			
		}
	}
</script>

<style>
.board-box-top swiper {
  height: 480rpx;
}

.board-box-top swiper image {
  height: 100%;
  width: 100%;
}
.board {
  box-sizing: border-box;
  background-color: #f8f9fb;
}

.board-item {
  display: flex;
  flex-direction: column;
  cursor: pointer;
  font-size: 20rpx;
  margin: 40rpx 0;
  padding: 20rpx;
  background-color: #fff;
}

.board-item .title {
  display: flex;
  margin-bottom: 10rpx;
  width: 100%;
}

.board-item .title text {
  flex: 1;
}

.board-item .title image {
  height: 20rpx;
  width: 20rpx;
}

.board-item .content {
  height: 300rpx;
}

.board-item .content .inner {
  display: flex;
  flex-direction: row;
  height: 300rpx;
  width: 900rpx;
}

.board-item .content .inner .movie-item {
  display: flex;
  flex-direction: column;
  width: 180rpx;
  margin: 10rpx;
}

.board-item .content .inner .movie-item image {
  width: 180rpx;
  height: 250rpx;
}

.board-item .content .inner .movie-item text {
  text-align: center;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
</style>