<template>
	<view class="wrapper">
		<cu-custom bgColor="bg-white" :isBack="true">
			<block slot="backText">茂南石化工业园区车辆管理平台</block>
		</cu-custom>
		
		<view class="header-wrap">
			<text class="date">{{workDate}}&nbsp;工单</text>
		</view>
		
		<view class="order-list-wrap">
			
			<block v-if="carList.length">
				
				<view class="order-list" v-for="(item,index) in carList" :key="index">
					<view class="order-info">
						<text class="number">{{item.sortNum}}</text>
						<text class="plate-number">{{item.licensePlate}}</text>
						<text class="mobile">{{item.mobile}}</text>
						<text class="way">{{item.workType == 1 ? "装油" : "卸油"}}</text>
					</view>
				</view>
			
			</block>
			
			<block v-else>
				<view class="tips">没有数据...</view>
			</block>
			
		</view>
			
			
	</view>
</template>

<script>
	export default {
		data() {
			return {
				workDate: '',
				carList: []
			}
		},
		onLoad(options) {

			this.workDate = options.workDate;
			
			this.carOrderList(options.orderSn);

		},
		methods: {
			async carOrderList(orderSn) {
				
				//根据工单号查询工单车辆列表请求
				let res = await uni.$http.get(uni.$url.queryCarListByOrderSnUrl, { orderSn });
				
				if (res.data.code !== 200) return uni.$showMsg(res.data.msg);
				
				this.carList = res.data.data;
				
			}
		}
	}
</script>

<style lang="scss">
	
	//头部
	.header-wrap {
		position: sticky;
		top: 45px;
		height: 80rpx;
		border-top: 1px solid #f1f1f1;
		background: #ffffff;
		z-index: 115;
		text-align: center;
		font-size: 34rpx;
		font-weight: 400;
		color: #000000;
		.date {
			line-height: 80rpx;
		}
	}
	
	.order-list-wrap {
		//已添加的列表
		.order-list {
			display: flex;
			width: 100%;
			padding: 50rpx 40rpx 50rpx 20rpx;
			margin-top: 10rpx;
			background: #FFFFFF;
			justify-content: space-between;
			align-items: center;
			.order-info {
				.number {
					display: inline-block;
					height: 50rpx;
					line-height: 50rpx;
					margin-right: 20rpx;
					padding: 0 20rpx;
					background: #EFEFEF;
					border-radius: 25rpx;
					font-size: 34rpx;
					font-weight: 400;
					color: #333333;
					text-align: center;
				}
		
				.plate-number {
					display: inline-block;
					height: 50rpx;
					line-height: 50rpx;
					padding: 2rpx 12rpx 0 12rpx;
					margin-right: 15rpx;
					background: #FFC600;
					border-radius: 10rpx;
					font-size: 34rpx;
					font-weight: bold;
					color: #000000;
					vertical-align: middle;
					
				}
		
				.mobile {
					font-size: 30rpx;
					font-weight: 400;
					color: #000000;
				}
				
				.way {
					margin-left: 20rpx;
					font-size: 30rpx;
					font-weight: 400;
					color: #999999;
				}
			}
		
			.order-handler {
				width: 110rpx;
				text-align: right;
				.icon-edit {
					display: inline-block;
					width: 30rpx;
					height: 30rpx;
				}
			}
		}
	}
	
	.tips {
		margin-top: 50%;
		text-align: center;
	}
</style>
