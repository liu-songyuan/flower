<template>
	<view class="user">
		<!-- 头部 -->
		<view class="user-wrap">
			<view class="info">
				<image class="avatar" mode="aspectFill" :src="userInfo.portrait || '/static/missing-face.png'"></image>
				<view class="info-box">
					<text class="nickname">{{userInfo.nickname || '游客'}}</text>
				</view>
			</view>
		</view>
		<!-- 订单状态 -->
		<view class="order-status">
			<view class="status-wrap">
				<!-- 单元格 -->
				<!-- 订单状态 -->
				<view class="status-list">
					<view class="status-item" hover-class="btn-hover" v-for="(item, index) in myStatusList" :key="index">

						<view class="item-text">{{ item.icon}}</view>
						<view class="item-text">{{ item.name }}</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 滑动导航 -->
		<view style="border-radius: 20rpx; overflow: hidden; margin: 0 20rpx;">
			<com-nav :col="5"></com-nav>
		</view>

		<!-- 用户功能 -->

		<view class="order-status">
			<view class="status-wrap">
				<!-- 单元格 -->
				<view class="cell">
					<view class="cell-left">
						<view class="cell-text wode">我的订单</view>
						<text class="look">查看全部订单</text>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
				<!-- 订单状态 -->
				<view class="status-list">
					<view class="status-item" hover-class="btn-hover" v-for="(item, index) in orderStatusList" :key="index" @click="navTo('/pages/order/order?state=(index)')">
						<view class="item-icon" :class="item.icon"></view>
						<view class="item-text">{{ item.name }}</view>
					</view>
				</view>
			</view>
		</view>


		<!-- 用户服务 -->
		<view class="com-item">
			<view class="com-wrap">
				<view class="cell" v-for="(item, index) in serverList" :key="index" @click="cartIndex(index)">
					<view class="cell-left">
						<image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
						<view class="cell-text" @click="cartIndex(index)">{{ item.title }}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item">
			<view class="com-wrap">
				<view @click="addressindex" class="cell" v-for="(item, index) in serverList1" :key="index">
					<view class="cell-left">
						<image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
						<view @click="addressindex" class="cell-text">{{ item.title }}</view>
					</view>
					<view class="iconfont iconmore1"></view>
				</view>
			</view>
		</view>
		<view class="com-item">
			<view class="com-wrap">
				<view @click="setIndex(index)" class="cell" v-for="(item, index) in serverList2" :key="index">
					<view@click="setIndex(index)" class="cell-left">
						<image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
						<view @click="setIndex(index)" class="cell-text">{{ item.title }}</view>
				</view>
				<view class="iconfont iconmore1"></view>
			</view>
		</view>
	</view>
	<view class="com-item">
		<view class="com-wrap">
			<view class="cell" v-for="(item, index) in serverList3" :key="index">
				<view class="cell-left">
					<image class="cell-icon" :src="item.icon" mode="aspectFill"></image>
					<view @click="marketIndex" class="cell-text">{{ item.title }}
						<text @click="marketIndex" class="dianpu">进入店铺</text>
					</view>
				</view>
				<view class="iconfont iconmore1"></view>
			</view>
		</view>
	</view>
	</view>
</template>

<script>
	import listCell from '@/components/mix-list-cell';
	import comNav from './components/com-nav.vue'
	import {
		mapState
	} from 'vuex';
	let startY = 0,
		moveY = 0,
		pageAtTop = true;
	export default {
		components: {
			comNav
		},
		components: {
			listCell
		},

		data() {
			return {
				// list: "",
				// userInfo: {
				// 	headPicUrl: '/static/images/user/avatar.jpg',
				// 	nickName: 'kiki'
				// },
				myStatusList: [{
						name: '余额',
						icon: '0.00'
					},
					{
						name: '积分',
						icon: '0'
					},
					{
						name: '卡',
						icon: '0'
					},
					{
						name: '优惠券/码',
						icon: '0'
					},
					{
						name: '零钱',
						icon: '0.00'
					}
				],
				orderStatusList: [{
						name: '待付款',
						icon: 'iconfont icon31daifukuan',
						status: 10
					},
					{
						name: '待发货',
						icon: 'iconfont icon31daifahuo',
						status: 30
					},
					{
						name: '待收货',
						icon: 'iconfont icon31daishouhuo',
						status: 40
					},
					{
						name: '评价',
						icon: 'iconfont icon31daipingjia',
						status: 50
					},
					{
						name: '退款/售后',
						icon: 'iconfont icon31yiguanzhudianpu',
						status: 55
					}
				],
				serverList: [{
						title: '购物车',
						icon: '/static/images/user/icon-kefu.png',
					},
					{
						title: '任务中心',
						icon: '/static/images/user/renwu.png',

					},
					{
						title: '赠品',
						icon: '/static/images/user/zengpin.png',

					}
				],
				serverList1: [{
					title: '收获地址',
					icon: '/static/images/user/dizhi.png',

				}],
				serverList2: [{
						title: '客服聊天',
						icon: '/static/images/user/liaotian.png',

					},
					{
						title: '个人信息',
						icon: '/static/images/user/geren.png',
					},
					{
						title: '账号设置',
						icon: '/static/images/user/shezhi.png',
					}
				],
				serverList3: [{
					title: '花花万物',
					icon: '/static/images/user/dianpu.png',
				}]
			};
		},

		computed: {
			...mapState(['hasLogin', 'userInfo'])
		},
		methods: {
			navTo(url) {
				if (!this.hasLogin) {
					url = '/pages/public/public';
				}
				uni.navigateTo({
					url
				})
			},
			addressindex() {
				uni.navigateTo({
					url: "/pages/address/address"
				})
			},
			marketIndex() {
				uni.reLaunch({
					url: "/pages/index/index"
				})
			},
			cartIndex(index) {
				if (index == 0) {
					uni.reLaunch({
						url: "/pages/carts/carts"
					})
				}
			},
			setIndex(index) {
				if (index == 2) {
					uni.navigateTo({
						url: "/pages/set/set"
					})
				}
			},
		}
	};
</script>

<style lang="scss">
	.dianpu {
		margin-left: 360rpx;
	}

	page {
		height: 1570rpx;
		background: #bcce96;
	}

	.btn-hover {
		background: #f2f2f2 !important;
	}

	.user {
		.user-wrap {
			display: flex;
			justify-content: center;
			align-items: center;
			height: 60vw;
			padding: 30rpx;
			z-index: 9;
			border-radius: 0 0 5% 5%;
			background: url('http://img.yzcdn.cn/upload_files/2020/03/22/FkeSLPLhfDbSYp0L0iXtUzP6H7k0.jpeg') no-repeat;
			background-size: cover;

			.setting {
				color: #fff;
				top: 60rpx;
				left: 60rpx;
				font-size: 50rpx;
			}

			.info {
				text-align: center;

				.avatar {
					width: 150rpx;
					height: 150rpx;
					border-radius: 50%;
				}

				.nickname {
					color: #fff;
					font-size: 35rpx;
				}
			}
		}

		.order-status {
			padding: 0 20rpx;
			/* #ifdef H5 */
			margin-top: 2vw;
			/* #endif */

			/* #ifndef H5 */
			margin-top: -15vw;
			/* #endif */

			.status-wrap {
				border-radius: 15rpx;
				overflow: hidden;

				.status-list {
					display: flex;
					justify-content: space-evenly;
					align-items: center;
					background: #fff;
					padding-top: 20rpx;
					padding-bottom: 30rpx;
					z-index: 99;

					.status-item {
						flex: 1;
						display: flex;
						flex-direction: column;
						justify-content: center;
						align-items: center;

						.item-icon {
							line-height: 1;
							font-size: 40rpx;
							color: #000000;
						}

						.item-text {
							font-size: 27rpx;
							color: #000000;
							margin-top: 5rpx;
						}
					}
				}
			}
		}

		.com-item {
			padding-left: 20rpx;
			padding-right: 20rpx;
			margin-top: 20rpx;

			.com-wrap {
				border-radius: 20rpx;
				overflow: hidden;
			}
		}

		.cell {
			height: 80rpx;
			padding-left: 20rpx;
			padding-right: 20rpx;
			display: flex;
			justify-content: space-between;
			align-items: center;
			background: #fff;
			border-bottom: 1px solid #f0f0f0;

			&:active {
				background: #f2f2f2;
			}

			&:last-child {
				border-bottom: none !important;
			}

			.cell-left {
				display: flex;
				align-items: center;

				.cell-icon {
					width: 35rpx;
					height: 35rpx;
				}

				.cell-text {
					color: #333333;
					font-size: 27rpx;
					margin-left: 15rpx;
				}

				.wode {
					color: #000000;
					font-weight: 670;
					font-size: 34rpx;
				}

				.look {
					font-size: 25rpx;
					margin-left: 330rpx;
				}
			}

			.iconfont {
				color: #040404;
			}
		}
	}
</style>
