<template>
	<view class="warp">
		<view class="top_change">
			<view class="t_c_text">商品库存有限，请尽快下单</view>
			<view class="t_c_change" @tap="onChange">{{isChange?'完成':'编辑'}}</view>
		</view>
		<view v-if="goods.length > 0" class="cen_goods">
			<view class="goods_text">
				<label class="radio" @tap="selectAll">
					 <radio 
						style="transform:scale(0.8);margin-left: 20rpx;" 
						color="#FF4544" 
						:checked="selectAllGoods" 
					/>
					<text>XXX 品牌</text>
				</label>
			</view>
			<view 
				class="goods_item"
				v-for="(item,index) in goods"
				:key="index"
			>
				<view class="lef_select">
					<label class="radio" @tap="selectGoods(index)">
						<radio 
							style="transform:scale(0.8)" 
							color="#FF4544" 
							:checked="item.selected" 
						/>
					</label>
				</view>
				<view class="rig_goods">
					<image :src="item.img" @tap="goodDetail(item.id)"></image>
					<view class="r_goods_rig">
						<view class="r_g_r_name">{{item.name}}</view>
						<view class="r_g_r_type">女装：26 码</view>
						<view class="r_g_r_price">
							<view class="r_g_r_p_price">￥{{item.price}}</view>
							<view class="r_g_r_p_num">
								<uni-number-box @change="changeNum(index)" :min="1" :max="100" v-model="item.num"></uni-number-box>
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view v-else class="cen_goods">
			<view class="no_goods">
				<image src="/static/images/common/no.png"></image>
				<text>购物车还是空的哦</text>
			</view>
		</view>
		<view class="bot_all_price">
			<view class="b_a_p_lef">
				<label class="b_a_p_l_radio" @tap="selectAll">
					 <radio 
						style="transform:scale(0.8);" 
						color="#FF4544" 
						:checked="selectAllGoods" 
					/>全选
				</label>
				<view v-if="!isChange" class="b_a_p_l_price">
					总计：￥{{totalPrice}}
				</view>
			</view>
			<view v-if="!isChange" @tap="goNowBuy" class="b_a_p_rig" :class=" totalPrice > 0?'btn_active':'' ">去结算</view>
			<view v-else class="b_a_p_rig btn_active" @tap="clearGoods">删除</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				selectAllGoods:false,
				goods:[
					{
						id:111,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:1,
						selected:false,
					},
					{
						id:222,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:10,
						selected:false,
					},
					{
						id:333,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:3,
						selected:false,
					},
					{
						id:444,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:1,
						selected:false,
					},
					{
						id:555,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:2,
						selected:false,
					},
					{
						id:666,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:1,
						selected:false,
					},
					{
						id:777,
						name:'jk制服裙正版夏季短袖衬衫格裙套装女学生学院风格全套百搭百褶裙',
						img:'/static/images/index/good.jpg',
						price:'215.26',
						num:1,
						selected:false,
					},
					
				],
				totalPrice:'0.00',
				isChange:false,
			};
		},
		methods:{
			goodDetail(id){
				uni.navigateTo({
					url:'/pages/goodDetail/goodDetail?id=' + id
				})
			},
			// 删除
			clearGoods(){
				let goods = this.goods;
				let clearGoods = [];
				for(let i = 0; i<goods.length; i++) {         
					if(goods[i].selected) {                   
						clearGoods.push(goods[i].id)   
					}
				};
				if(clearGoods.length <= 0){
					uni.showToast({
						title:'请选择商品',
						icon:'none'
					})
				}
				console.log('删除的商品--',clearGoods)
			},
			// 编辑
			onChange(){
				this.isChange = !this.isChange;
			},
			goNowBuy(){
				let goods = this.goods;
				let buyGoods = [];
				for(let i = 0; i<goods.length; i++) {         
					if(goods[i].selected) {                   
						buyGoods.push(goods[i].id)   
					}
				};
				console.log(buyGoods)
				uni.navigateTo({
					url:'/pages/nowBuy/nowBuy?id='+JSON.stringify(buyGoods)
				})
			},
			// 全选
			selectAll(){
				let selectAllGoods = this.selectAllGoods;
				selectAllGoods = !selectAllGoods;
				let goods = this.goods;
				for (let i = 0; i < goods.length; i++) {
					goods[i].selected = selectAllGoods;
				}
				this.selectAllGoods = selectAllGoods;
				this.goods = goods;
				this.getTotalPrice();              
			},
			// 选择商品
			selectGoods(index){
				const selected = this.goods[index].selected;
				this.goods[index].selected = !selected;
				this.getTotalPrice();
				if(selected){
					this.selectAllGoods = false;
				}else{
					const goods = this.goods;
					let selectAll = 0;
					goods.forEach(item => {
						if(!item.selected){
							selectAll = 1;
						}
					})
					if(selectAll == 0){
						this.selectAllGoods = true;
					}
				}
			},
			// 加减数量
			changeNum(index){
				this.$nextTick(function() {
					this.getTotalPrice()
				});
			},
			// 获取总价
			getTotalPrice(){
				let goods = this.goods;                 
				let total = 0;
				for(let i = 0; i<goods.length; i++) {         
					if(goods[i].selected) {                   
						total += goods[i].num * goods[i].price;   
					}
				};
				this.totalPrice = total.toFixed(2);
			}
		}
	}
</script>

<style lang="scss" scoped>
.warp{
	width: 100%;
	height: 100vh;
	background-color: #F7F7F7;
	display: flex;
	flex-direction: column;
	.top_change{
		width: 100%;
		height: 80rpx;
		background-color: white;
		margin-bottom: 20rpx;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		.t_c_text{
			font-size: 26rpx;
			color: #a7a7a7;
			margin-left: 25rpx;
		}
		.t_c_change{
			margin-right: 25rpx;
			font-size: 28rpx;
			color: #3f3f3f;
		}
	}
	.cen_goods{
		width: 100%;
		height: calc(100vh - 201rpx);
		overflow-y: auto;
		display: flex;
		flex-direction: column;
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
		.goods_text{
			width: 100%;
			height: 100rpx;
			background-color: white;
			display: flex;
			flex-direction: row;
			align-items: center;
			.radio{
				width: 100%;
				height: 100%;
				display: flex;
				flex-direction: row;
				align-items: center;
				color: #6a6a6a;
				font-size: 27rpx;
				text{
					margin-left: 10rpx;
				}
			}
		}
		.goods_item{
			width: 100%;
			height:250rpx;
			border-top: 1rpx solid #f0f0f0;
			background-color: white;
			display: flex;
			flex-direction: row;
			justify-content: space-between;
			align-items: center;
			.lef_select{
				width: 100rpx;
				height: 100%;
				display: flex;
				flex-direction: row;
				align-items: center;
				justify-content: center;
			}
			.rig_goods{
				width: 620rpx;
				height: 100%;
				display: flex;
				flex-direction: row;
				justify-content: space-between;
				align-items: center;
				margin-right: 30rpx;
				image{
					height: 150rpx;
					width: 150rpx;
				}
				.r_goods_rig{
					width: 450rpx;
					display: flex;
					flex-direction: column;
					// border: 1rpx solid red;
					.r_g_r_name{
						width: 100%;
						font-size:32rpx;
						color: #4c4c4c;
						overflow: hidden;
						text-overflow:ellipsis;
						white-space: nowrap;
					}
					.r_g_r_type{
						font-size:29rpx;
						margin-top: 10rpx;
						color: #7d7d7d;
					}
					.r_g_r_price{
						width: 100%;
						margin-top: 20rpx;
						display: flex;
						flex-direction: row;
						align-items: center;
						justify-content: space-between;
						.r_g_r_p_price{
							color: #FF4544;
						}
					}
				}
			}
		}
	}
	.bot_all_price{
		width: 100%;
		height: 100rpx;
		background-color: white;
		border-top: 1rpx solid #f0f0f0;
		display: flex;
		flex-direction: row;
		align-items: center;
		justify-content: space-between;
		.b_a_p_lef{
			display: flex;
			flex-direction: row;
			align-items: center;
			margin-left: 20rpx;
			.b_a_p_l_radio{
				display: flex;
				flex-direction: row;
				flex-wrap: nowrap;
				align-items: center;
			}
			.b_a_p_l_price{
				margin-left: 20rpx;
				color: #FF4544;
			}
		}
		.b_a_p_rig{
			width: 200rpx;
			height: 70rpx;
			border-radius: 40rpx;
			display: flex;
			align-items: center;
			justify-content: center;
			background-color: #989898;
			font-size: 29rpx;
			color: #DCDCDC;
			margin-right: 20rpx;
		}
		.btn_active{
			background-color:  #FF4544;
			color: white;
		}
	}
}
</style>
