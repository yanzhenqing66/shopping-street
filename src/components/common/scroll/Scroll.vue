<template>
	<div class="wrapper" ref="wrapper">
		<div class="content">
			<slot></slot>
		</div>
	</div>
</template>

<script>
	import BScroll from "better-scroll";

	export default {
		name: "Scroll",
		props: {
			probeType: {
				type: Number,
				default: 0
			},
			pullUpLoad: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				scroll: null
			};
		},
		mounted() {
			// 创建 BScroll 对象
			this.scroll = new BScroll(this.$refs.wrapper, {
				click: true,
				probeType: this.probeType,
				pullUpLoad: this.pullUpLoad
			});
			// 监听滚动位置
			this.scroll.on("scroll", position => {
				this.$emit("scroll", position);
			});
			// 上滑加载更多，监听滚到底部
			this.scroll.on("pullingUp", () => {
				this.$emit("pullingUp");
			});
		},
		methods: {
			scrollTo(x, y, time = 300) {
				this.scroll && this.scroll.scrollTo(x, y, time);
			},
			finishPullUp() {
				this.scroll && this.scroll.finishPullUp()
			},
			// 刷新数据
			refresh() {
				// console.log('asd')
				this.scroll && this.scroll.refresh()
			},
			getScrollY() {
				return this.scroll ? this.scroll.y : 0
			}
		}
	};
</script>

<style scoped>
</style>
