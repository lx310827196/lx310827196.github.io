<template>
	<view>
		<view class="lucky-draw" :style="scale">
			<image class="lucky-draw__bg" src="@/static/images/background.png" mode="widthFix" style="width: 700rpx;"></image>
			
			<view class="lucky-draw__objects" :style="rotateStyle">
				<image src="@/static/images/objects-bg.png" class="lucky-draw__objects__bg"></image>
				<view class="lucky-draw__objects__object" v-for="(item, index) in dataA" :key="index" :style="{
		      transform: `translate(-50%) rotate(${(360 / 8) * index}deg)`,
		    }">
			
					<view class="lucky-draw__objects__object__title">{{ item.name }}</view>
					<!-- <image class="lucky-draw__objects__object__icon" :src="item.reward_image"></image> -->
				</view>
			</view>
			<image class="lucky-draw__start" src="@/static/images/start.png" @click="starta()"></image>
		</view>
		<viwe style="font-size: 40rpx;font-weight: 600;">制作者：王从彦</viwe>
	</view>
</template>

<script>
	export default {
		name: "lucky-draw",
		data() {
			return {
				dataA: [{
						amount: "0",
						id: 1,
						name: "谢谢参与",
						probability: 10,
						// reward_image: "/static/images/ces.png",
						type: "1"
					},
					{
						amount: "200",
						id: 2,
						name: "三等奖",
						probability: 20,
						// reward_image: "/static/images/ces.png",
						type: "2"
					},
					{
						amount: "0",
						id: 3,
						name: "二等奖",
						probability: 1,
						// reward_image: "/static/images/ces.png",
						type: "1"
					},
					{
						amount: "0",
						id: 4,
						name: "三等奖",
						probability: 80,
						// reward_image: "/static/images/ces.png",
						type: "2"
					},
					{
						amount: "0",
						id: 5,
						name: "三等奖",
						probability: 0,
						// reward_image: "/static/images/ces.png",
						type: "1"
					},
					{
						amount: "0",
						id: 6,
						name: "三等奖",
						probability: 1,
						// reward_image: "/static/images/ces.png",
						type: "1"
					},
					{
						amount: "0",
						id: 7,
						name: "二等奖",
						probability: 0,
						// reward_image: "/static/images/ces.png",
						type: "1"
					},
					{
						amount: "0",
						id: 8,
						name: "一等奖",
						probability: 0,
						// reward_image: "/static/images/ces.png",
						type: "1"
					}
				],
				isStart: true,//为了不连续点击
				rotateAngle: 0, // 旋转角度
				cycle: 1, // 第几次抽奖
				ida:0,
				ids:0
			};
		},
		computed: {
			rotateStyle() {
				return `
        -webkit-transition: transform ${this.duration}ms ${this.mode};
        transition: transform ${this.duration}ms ${this.mode};
        -webkit-transform: rotate(${this.rotateAngle}deg);
            transform: rotate(${this.rotateAngle}deg);`;
			},
			scale() {
				return `transform: scale(${this.size / 686})`;
			},
		},
		props: {
			duration: {
				// 总旋转时间 ms级
				type: Number,
				default: 5000,
			},
			circle: {
				// 旋转圈数
				type: Number,
				default: 5,
			},
			mode: {
				// 由快到慢 惯性效果都省了
				type: String,
				default: "cubic-bezier(0.2, 0, 0, 1)",
			},
			size: {
				type: Number,
				default: 686,
			},
			config: {
				type: Object,
				default: () => ({}),
			},
			type: {
				type: Number,
				default: 0,
			},
		},
		created() {
			
		},
		methods: {
			starta() {
				
				if(this.isStart){
					this.ida++;
					this.ids++;
					if(this.ids > 8){
						this.ida = 1;
					}
					// var ida = Math.ceil(Math.random()*10);
					//这里我是写的一个随机数。
					//当后台返回我所获得的奖项id，根据id和数组里的比较，相等则旋转到奖品角度
					this.isStart = false;
					this.rotateAngle =
								this.circle * 360 * this.cycle -
								(360 / 8 +
									(this.dataA.findIndex((item) => item.id == this.ida) -
										1) *
									(360 / 8));
							this.cycle++;
							setTimeout(() => {
								this.dataA.forEach((item)=>{
									if(item.id == this.ida ){
										uni.showToast({
											// title: '恭喜您获得' + item.name,
											title: item.name,
											icon: "none"
										})
										this.isStart = true;
										this.$emit("finish");//向父组件发送结束事件
									}
								})
							}, 6000)
				}
			},
		},
	};
</script>
<style scoped lang="scss">
	.lucky-draw {
		position: relative;
		width: fit-content;

		&__bg {
			width: 686rpx;
			display: block;
		}

		&__lights {
			position: absolute !important;
			width: 676rpx;
			left: 6rpx;
			top: 10rpx;
			display: block;
			animation: infinite 1s alternate lights steps(1, end);
		}

		&__objects {
			position: absolute;
			top: 73rpx;
			left: 113rpx;
			right: 63rpx;
			bottom: 78rpx;
			width: 500rpx;
			height: 500rpx;

			&__bg {
				position: absolute;
				width: 500rpx;
				height: 500rpx;
			}

			&__object {
				position: absolute;
				left: 50%;
				transform: translateX(-50%);
				top: 0;
				transform-origin: 50% 250rpx;

				&__title {
					margin-top: 62rpx;
					color: #4E0691;
					font-size: 20rpx;
					-webkit-transform: scale(0.7);
				}

				&__icon {
					width: 52rpx;
					height: 52rpx;
					display: block;
					margin: 18rpx auto 0;
				}
			}
		}

		&__start {
			width: 150rpx;
			height: 170rpx;
			display: block;
			position: absolute;
			left: 290rpx;
			top: 225rpx;
		}
	}

	@keyframes lights {
		0% {
			transform: rotate(0deg);
		}

		50% {
			transform: rotate(15deg);
		}

		100% {
			transform: rotate(0deg);
		}
	}
</style>