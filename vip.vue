<!-- 搜索 -->
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
		<!-- 商品内容 -->
		<yd-infinitescroll :callback="loadData" ref="infinitescrollDemo">
	        <yd-list theme="1" slot="list" class="content">
	            <router-link :to="'/goodDetail?isVip=true&id='+item.id" class="good" v-for="item in list" :key="item.id">
		    		<img :src="item.logo" class="good-img">
		    		<p class="name">{{item.name}}</p>
		    		<div class="price">¥{{item.univalent}}</div>
		    	</router-link>
	        </yd-list>


	        <!-- 数据全部加载完毕显示 -->
	        <span slot="doneTip">啦啦啦，啦啦啦，没有数据啦~~</span>

	        <!-- 加载中提示，不指定，将显示默认加载中图标 -->
	        <img slot="loadingTip" src="http://static.ydcss.com/uploads/ydui/loading/loading10.svg"/>

	    </yd-infinitescroll>
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
				list: [],
				page: 1,
			}
		},
		created(){
            this.loadBanner();
            this.loadData();
		},
		methods:{
			loadData() {//加载数据
				let self = this;
				self.$dialog.loading.open('数据加载中');
				util.load("post","shop_detail",{
					page: this.page,
				},function(result){
					self.$dialog.loading.close();
					self.shop_list = result.data;
					let _list = result.data;

                    self.list = [...self.list, ..._list];

                    if (_list.length < 10) {
                        /* 所有数据加载完毕 */
                        self.$refs.infinitescrollDemo.$emit('ydui.infinitescroll.loadedDone');
                        return;
                    }

                    /* 单次请求数据完毕 */
                    self.$refs.infinitescrollDemo.$emit('ydui.infinitescroll.finishLoad');

                    self.page++;
	        	});
            },
			loadBanner(){//获取banner
				let self = this;
				util.load("post","getBanner",{
					type: 2
				},function(result){
					self.banner = result.data;
	        	});
			},
		}
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
		.content{
			display: flex;
			flex-wrap: wrap;
			justify-content: space-between;
			background-color: #fff;
			padding: 0.2rem;

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