<!-- 首页 -->
<template>
	<yd-layout class="index">
		<div class="top" slot="navbar">
			<router-link to="/search" class="search">
				<yd-icon name="search" size="0.4rem" color="#fff" custom></yd-icon>
			</router-link>
		</div>
		<!-- 轮播 -->
		<yd-slider autoplay="3000" style="height:3.2rem">
	        <yd-slider-item v-for="item in banner">
                <img :src="item.banner_url">
	        </yd-slider-item>
	    </yd-slider>
	    <!--  -->
	    <div class="tab-menu">
	    	<router-link to="/culture" class="menu">
	    		<img src="./../../static/image/culture.png" class="tab-icon">
	    		<p>硒文化</p>
	    	</router-link>
	    	<!-- <router-link to="/shopMall" class="menu"> -->
	    	<div @click="noopen" class="menu">
	    		<img src="./../../static/image/mall.png" class="tab-icon">
	    		<p>硒商城</p>
	    	</div>
	    	<!-- </router-link> -->
	    	<router-link to="/video" class="menu">
	    		<img src="./../../static/image/video.png" class="tab-icon">
	    		<p>宣传视频</p>
	    	</router-link>
	    	<router-link to="/weal" class="menu">
	    		<img src="./../../static/image/weal.png" class="tab-icon">
	    		<p>福利专区</p>
	    	</router-link>
	    </div>	
	    <div>
		    <!-- notice -->
		    <div class="notice-box">
			    <yd-rollnotice autoplay="2000" class="notice">
			        <yd-rollnotice-item v-for="item in notice">
			        	<yd-icon name="msg" size="0.3rem" color="#999" custom></yd-icon>
			        	{{item.content}}
			    	</yd-rollnotice-item>
			    </yd-rollnotice>
			    <router-link to="/notice" class="more">
			    	查看更多》
			    </router-link>
		    </div>
		    <div class="content">
		    	<!-- <router-link :to="'/goodDetail?id='+item.id" class="good" v-for="item in shop_list" :key="item.id">
		    		<img :src="item.logo" class="good-img">
		    		<p class="name">{{item.name}}</p>
		    		<div class="price">¥{{item.univalent}}</div>
		    	</router-link> -->

		    	<!-- 展示公告内容一条 -->
		    	<div v-html="culture.article_text" class="info"></div>
		    </div>
	    </div>

		<foot-nav slot="tabbar"></foot-nav>
	</yd-layout>
</template>
<script>
	import util from "util"
	import footNav from './sub/footnav';
	export default{
		components: {
          footNav,
      	},
		data(){
			return {
				banner:[],//顶部banner 数据
				notice: [],//公告
				shop_list: [],//推荐商品
				culture:{}
			}
		},
		created(){
			let self = this;
            this.loadBanner();
            this.loadNotice();
            this.loadRecom();
            util.load("post","cultrue_detail",{
				id: 8
			},function(result){
				self.culture = result.data
				setTimeout(function(){
	                $(".info img").css("max-width","100%");
	            }, 500);
        	});
        	util.wxConfig();
		},
		methods:{
			noopen(){
				this.$dialog.toast({
                    mes: '商城正在开发中,敬请期待！',
                    timeout: 1500
                });
			},
			loadBanner(){//获取banner
				let self = this;
				util.load("post","getBanner",{
					type: 1
				},function(result){
					self.banner = result.data;
	        	});
			},
			loadNotice(){//获取公告
				let self = this;
				util.load("post","notification",{
					page: 1
				},function(result){
					self.notice = result.data;
	        	});
			},
			loadRecom(){//获取推荐
				let self = this;
				self.$dialog.loading.open('数据加载中');
				util.load("post","shop_list",{
					page: 1
				},function(result){
					self.$dialog.loading.close();
					self.shop_list = result.data;
	        	});
			},
		},
	}
</script>
<style lang="less" scoped>
	@import '~assets/less/varibles.less';
	.index{
		
		.top{
			height: 0.9rem;
			background-color: @main_color;

			.search{
				display: flex;
				align-items: center;
				height: 0.5rem;
				line-height: 0.5rem;
				text-align: left;
				padding-left: 0.2rem;
				margin: 0.2rem;
				background-color: rgba(255,255,255,0.4);
				border-radius: 0.25rem;
			}
		}
		.tab-menu{
			display: flex;
			justify-content: space-between;
			align-items: center;
			margin: 0.2rem;
			padding: 0.2rem;
			box-shadow: 0 0 0.1rem #acf595;
			background-color: #fff;
			border-radius: 0.1rem;

			.menu{
				line-height: 0.5rem;
				font-size: @main_medium_size;
				color: #333;
				
				.tab-icon{
					width: 1rem;
					height: 1rem;
					border-radius: 50%;
					box-shadow: 0.01rem 0.01rem 0.08rem #000000;
				}
			}
		}
		.service{
			position: fixed;
			bottom: 4rem;
			right: 0.3rem;
			display: block;
			background-color: #fff;
			box-shadow: 0 0 0.1rem #acf595;
			border-radius: 50%;
			padding: 0.1rem;
		}
		.notice-box{
			display: flex;
			justify-content: space-between;
			align-items: center;
			padding: 0 0.3rem;
			font-size: @main_small_size ;
			background-color: #fff;

			.notice{
				height: 0.8rem;
				line-height: 0.8rem;
				overflow:hidden;
			    text-overflow:ellipsis;
			    white-space:nowrap
			}
			.more{
				width: 2rem;
				color: @color-main-gray;
			}
		}
		
		.content{
			display: flex;
			flex-wrap: wrap;
			justify-content: space-between;
			background-color: #fff;
			padding: 0.2rem;
			padding-bottom: 1.2rem;

			.good{
				width: 3.5rem;
				line-height: 0.45rem;
				font-size: @main_medium_size;
				border-radius: 0.1rem;
				border: 0.01rem solid #eaeaea;
				margin-bottom: 0.2rem;
				padding-bottom: 0.2rem;
				overflow: hidden;
				box-shadow: 0.02rem 0 0.05rem #d6d6d6;

				.good-img{
					width: 100%;
					height: 3.5rem;
				}
				.name{
					text-align: left;
					padding: 0 0.2rem;
					overflow: hidden;
					text-overflow:ellipsis;
					white-space: nowrap;
				}
				.price{
					padding: 0 0.2rem;
					text-align: left;
					font-size: @main_size;
					color: #ff9400;
					font-weight: 600;
				}
			}
		}
	}
</style>