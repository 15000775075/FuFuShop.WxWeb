<template>
	<view class="warp">
		<view class="top_search">
			<view class="search" @tap="goSearch">
				<icon type="search" size="16" style="margin-right: 10rpx;"/> 搜索商品名称
			</view>
		</view>
		<view class="top_list">
			<view class="top_tab">
				<view 
					class="tab_list" 
					v-for="(item,index) in topNavList" 
					:key="index" 
					@tap="onTopTabs(index)"
				>	
					<view class="tab_item" :class="tabIndex==index?'active':''">{{item}}</view>
				</view>
			</view>
		</view>
		<view v-if="orders.length > 0" class="cen_goods">
			<view v-for="(item,index) in orders" :key="index" class="orders_item">
				<view class="item_good">
					<image :src="item.img" @tap="goGoodDetail(item.id)"></image>
					<view class="good_rig">
						<view class="good_rig_name">{{item.name}}</view>
						<view class="good_rig_category">{{item.category}}</view>
					</view>
				</view>
				<view class="item_price">
					共{{item.num}} 件商品  实付款：￥{{item.totalPrice}}
				</view>
				<view class="item_price">
					<view class="btn" @tap="onLookFp(item.id)">查看发票</view>
					<view class="btn" @tap="onEvaluate(item.id)">评价晒单</view>
					<view class="btn color1" @tap="goGoodDetail(item.id)">再次购买</view>
				</view>
			</view>
		</view>
		<view v-else class="cen_goods">
			<view class="no_goods">
				<image src="/static/images/common/no.png"></image>
				<text>暂无相关订单</text>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				topNavList:['全部','待付款','待发货','待收货','已完成'],
				tabIndex:0,
				orders:[
					{
						id:111,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:2,
						totalPrice:'430.32',
						category:'27黑色 （9956款）',
					},
					{
						id:111,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:2,
						totalPrice:'430.32',
						category:'27黑色 （9956款）',
					},
					{
						id:111,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:2,
						totalPrice:'430.32',
						category:'27黑色 （9956款）',
					},
				
				],
			};
		},
		onLoad(options) {
			this.tabIndex = options.tabIndex
		},
		methods:{
			onTopTabs(index){
				this.tabIndex = index;
			},
			goSearch(){
				uni.navigateTo({
					url:'/pages/order-search/order-search'
				})
			},
			goGoodDetail(id){
				uni.navigateTo({
					url:'/pages/goodDetail/goodDetail?id='+id
				})
			},
			onEvaluate(id){
				uni.navigateTo({
					url:'/pages/evaluate/evaluate?id='+id
				})
			},
			onLookFp(id){
				uni.navigateTo({
					url:'/pages/orders-fp/orders-fp?id='+id
				})
			}
		}
	}
</script>

<style lang="scss" scoped>
.warp{
	width: 100%;
	height: 100vh;
	background-color:#F7F7F7;
	display: flex;
	flex-direction: column;
	align-items: center;
	.top_search{
		width: 100%;
		height: 100rpx;
		background-color: #F2F2F2;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		.search{
			width: 700rpx;
			height: 70rpx;
			color:#a5a5a5;
			background-color: white;
			border: 1rpx solid #f5f5f5;
			border-radius: 10rpx;
			font-size: 26rpx;
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: center;
		}
	}
	.top_list{
		width: 100%;
		background-color: white;
		position: sticky;
		top: 0;
		display: flex;
		flex-direction: column;
		.top_tab{
			width: 100%;
			height: 100rpx;
			display: flex;
			flex-direction:row;
			align-items: center;
			justify-content: center;
			.tab_list{
				width: 18%;
				height: 100%;
				display: flex;
				flex-direction:row;
				align-items: center;
				justify-content: center;
				font-size: 29rpx;
				.tab_item{
					height:60rpx;
					width: 100%;
					border-radius: 35rpx;
					font-family: PingFangSC-Ultralight,ans-serif;
					display: flex;
					flex-direction: row;
					align-items: center;
					justify-content: center;
				}
				.active{
					color: white;
					background-color:#FF4544;
				}
			}
		}
	}
	.cen_goods{
		width: 100%;
		height: calc(100vh - 201rpx);
		overflow-y: auto;
		display: flex;
		flex-direction: column;
		align-items:center;
		// border: 1rpx solid red;
		.orders_item{
			margin-top: 20rpx;
			margin-bottom: 20rpx;
			width: 660rpx;
			background-color: white;
			border-radius: 10rpx;
			padding:  20rpx 20rpx 0rpx 20rpx;
			display: flex;
			flex-direction: column;
			.item_good{
				width: 100%;
				height: 200rpx;
				background-color: #F8F8F8;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: space-between;
				image{
					height: 150rpx;
					width: 150rpx;
				}
				.good_rig_name{
					width: 480rpx;
					font-size: 26rpx;
					color: #555555;
					display: -webkit-box;
					overflow: hidden;
					text-overflow: ellipsis;
					word-wrap: break-word;
					white-space: normal !important;
					-webkit-line-clamp: 2;
					-webkit-box-orient: vertical;
				}
				.good_rig_category{
					width: 480rpx;
					font-size: 26rpx;
					color: #555555;
					display: -webkit-box;
					overflow: hidden;
					text-overflow: ellipsis;
					word-wrap: break-word;
				}
			}
			.item_price{
				width: 100%;
				padding: 20rpx 0rpx 20rpx 0rpx;
				font-size: 29rpx;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: flex-end;
			}
			.btn{
				height: 50rpx;
				width: 160rpx;
				background-color: #F8F8F8;
				font-size: 26rpx;
				border-radius: 30rpx;
				display: flex;
				color: #474747;
				align-items: center;
				justify-content: center;
				border: 1rpx solid #DCDCDC;
				margin-right: 20rpx;
			}
			.color1{
				border: 1rpx solid red;
				color: red;
			}
		}
		.no_goods{
			display: flex;
			flex-direction: column;
			align-items: center;
			margin: 0 auto;
			font-size: 30rpx;
			color: #9d9d9d;
			image{
				width: 400rpx;
				height: 400rpx;
				margin-bottom: 10rpx;
			}
		}
	}
}
</style>
