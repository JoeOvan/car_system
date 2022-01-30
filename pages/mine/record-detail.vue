<template>
	<view class="wrapper">
		<cu-custom bgColor="bg-white" :isBack="true">
			<block slot="backText">茂南石化工业园区车辆管理平台</block>
		</cu-custom>
		
		<view class="tab-menu-wrap">
			<view class="tab-menu">
				<view class="swiper-item">
					{{workDate}}&nbsp;&nbsp;工单
				</view>
			</view>
		</view>
		
		<view class="tab-content">
			<view class="swiper-item">
				
				<block v-if="carList && carList.length >= 1">
				
					<block  v-for="(item, index) in carList" :key="index">
						
						<view class="order-list-wrap">


							<block v-if="item.isViolation == 0">								
								<view class="carbar">
									<view class="car-info">
										<text class="number">{{index + 1}}</text>
										<text class="plate-number">{{item.licensePlate}}</text>
										<text class="mobile">{{item.mobile}}</text>
										<text class="way">{{item.workType == 1 ? '装油' : '卸油'}}</text>
									</view>
									<view class="car-job-time">
										<view class="job-time">
											{{item.durationTime || '无时间'}}
										</view>
										<view class="scope">
											{{item.inputTime || '无时间'}} -- {{item.outTime || '无时间'}}
										</view>
									</view>
								</view>
							</block>

							<block v-else>
								
								<view class="carbar">
									<view class="car-info">
										<text class="number">{{index + 1}}</text>
										<text class="plate-number">{{item.licensePlate}}</text>
										<text class="mobile">{{item.mobile}}</text>
										<text class="way">{{item.workType == 1 ? '装油' : '卸油'}}</text>
									</view>
									<view class="car-job-time">
										<view class="job-time overtime">
											{{item.durationTime || '无时间'}}
										</view>
										<view class="scope">
											{{item.inputTime || '无时间'}} -- {{item.outTime || '无时间'}}
										</view>
									</view>
								</view>
								
							</block>

						</view>
						
					</block>
		
				</block>
				
				<block v-else>
					<view class="tips">
						没有作业记录了
					</view>
				</block>
					
			</view>
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
	.tab-menu-wrap {
		position: sticky;
		top: 45px;
		height: 80rpx;
		border-top: 1px solid #f1f1f1;
		background: #ffffff;
		z-index: 115;
		.tab-menu {
			height: 100%;
			.swiper-item {
				line-height: 80rpx;
				text-align: center;
			}
		}
		.tab-left {
			position: absolute;
			top: 0;
			left: 0;
			padding: 0;
			margin: 0;
			line-height: 1;
			background-color: #FFFFFF;
			border: none;
			&::after {
				content: '';
				border: none;
			}
			.icon-arrow-left {
				display: inline-block;
				width: 17rpx;
				height: 28rpx;
				padding: 22rpx 20rpx;
				box-sizing: content-box;
				border: none;
			}
		}
		.tab-right {
			position: absolute;
			top: 0;
			right: 0;
			padding: 0;
			margin: 0;
			line-height: 1;
			background-color: #FFFFFF;
			border: none;
			&::after {
				content: '';
				border: none;
			}
			.icon-arrow-right {
				display: inline-block;
				width: 17rpx;
				height: 28rpx;
				padding: 22rpx 20rpx;
				box-sizing: content-box;
				border: none;
			}
		}
	}
	
	
	//作业记录详情
	.carbar {
		display: flex;
		height: 130rpx;
		// line-height: 130rpx;
		width: 100%;
		margin-top: 10rpx;
		padding: 0 20rpx;
		justify-content: space-between;
		align-items: center;
		background: #FFFFFF;
		.car-info {
			width: 640rpx;
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
				height: 50rpx;
				line-height: 50rpx;
				padding: 6rpx 12rpx;
				margin-right: 15rpx;
				background: #FFC600;
				border-radius: 10rpx;
				font-size: 34rpx;
				font-weight: bold;
				color: #000000;
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
	
		.car-job-time {
			position: relative;
			display: flex;
			width: 200rpx;
			flex-direction: column;
			text-align: right;
			.job-time {
				font-size: 30rpx;
				font-weight: bold;
				color: #666666;
				&.overtime {
					color: #FE4D4C;
				}
			}
			.scope {
				display: inline-block;
				position: absolute;
				right: -10rpx;
				bottom: -30rpx;
				width: 480rpx;
				font-size: 20rpx;
				font-weight: 400;
				color: #888888;
				-webkit-transform: scale(0.95);
				-moz-transform: scale(0.95);
				-o-transform: scale(0.95);
				transform: scale(0.95);
			}
		}
	}
	
	.tips {
		padding: 50rpx;
		text-align: center;
	}
	
</style>
