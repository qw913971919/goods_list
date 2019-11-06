<template>
	<view class="goods_list">
		<view class="goods_item" v-for="(item,index) in goodslist" :key="index">
			<image :src="item.img_url" mode="widthFix"></image>
			<text class="title">{{item.title}}</text>
			<view class="content">
				<view class="price">
					<text class="sell_price">￥{{item.sell_price}}</text>
					<text class="market_price">￥{{item.market_price}}</text>
				</view>
				<view class="stock_quantity">
					<text>热卖中</text>
					<text>剩{{item.stock_quantity}}件</text>
				</view>
			</view>
		</view>
		<button @click="this.addgoodslist()">加载更多</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				// 商品信息
				goodslist:[],
				flag:1,
			}
		},
		onLoad() {
			this.getgoodsList(this.flag);
		},
		methods: {
			getgoodsList(index){
				// 页面加载前发送ajax获取数据
				uni.request({
					url:'http://www.liulongbin.top:3005/api/getgoods?pageindex='+index,
					success:res=>{
						if(res.data.status===0){
							this.goodslist=res.data.message
							console.log(this.goodslist)
						}
					}
				})
			},
			addgoodslist(){
				// 点击加载更多
				this.flag+=1;
				// 页码标识
				uni.request({
					url:'http://www.liulongbin.top:3005/api/getgoods?pageindex='+this.flag,
					success:res=>{
						if(res.data.status===0){
							this.goodslist=this.goodslist.concat(res.data.message)
							console.log(this.goodslist)
						}
					}
				})
			}
		}
	}
</script>

<style lang="scss">
.goods_list{
	// 两列布局
	width:100%;
	display: flex;
	flex-wrap:wrap;
	justify-content: space-between;
	button{
		height:50px;
		width:90%;
		margin-bottom: 5px;
		text-align: center;
	}
	.goods_item{
		width:44%;
		margin:3px 10px;
		border:1px solid #ccc;
		box-shadow: 0 0 4px #ccc;
		border-radius: 7px;
		display: flex;
		flex-direction: column;
		justify-content:space-around;
		image{
			height:140px;
			width:100%;
			flex:7;
		}
		.title{
			margin:2px;
			font-size:14px;
			word-break: break-all;
			text-overflow: ellipsis;
			display: -webkit-box;
			-webkit-box-orient: vertical;
			-webkit-line-clamp: 2;
			overflow: hidden;
			// 两行文字,多余省略

			}
		.content{
			margin:2px 3px;
			background-color:#C0C0C0;
			.price{
				display: flex;
				font-size: 14px;
				.sell_price{
					color:red;
					margin-right:10px;
					font-weight:bold;
				}
				.market_price{
				font-size: 13px;
				text-decoration: line-through;
				}
			}
			.stock_quantity{
				font-size: 14px;
				display: flex;
				justify-content: space-between;
			}
		}
	}
}
</style>
