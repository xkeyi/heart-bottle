<template>
	<view class="bottle" @touchmove="handleTouchMove" @touchstart="handleTouchStart" @touchend="handleTouchEnd">
	  <view class="bottle-neck"></view>
	  <view class="bottle-body">
	    <view class="bottle-empty" :style="{ height: empty + '%' }"></view>
	    <view class="bottle-full" :style="{ height: full + '%' }"></view>
	  </view>
	</view>
</template>

<script>
	export default {
    name: 'bottle',
		data() {
			return {
				flag: 0,
				lastX: 0,
				lastY: 0,
        full: 0,
        empty: 100
			}
		},
		methods: {
			handleTouchMove(event) {
        if (this.flag !== 0) {
          return
        }
        
        let currentX = event.touches[0].pageX;
        let currentY = event.touches[0].pageY;
        let tx = currentX - this.lastX;
        let ty = currentY - this.lastY;
        
        this.flag = 1
        if (Math.abs(tx) > Math.abs(ty)) { //左右方向滑动
           //
        } else { //上下方向滑动
          if (ty < 0) {
            // 向上滑动
             let full = this.full + Math.abs(ty)/5*15
             this.full = full > 100 ? 100 : full
          } else if (ty > 0) {
            // 向下滑动
            let full = this.full - Math.abs(ty)/5*15
            this.full = full < 0 ? 0 : full
          }
          
          this.empty = 100 - this.full
        }

        //将当前坐标进行保存以进行下一次计算
        this.lastX = currentX;
        this.lastY = currentY;
      },
      handleTouchStart(event) {
          this.lastX = event.touches[0].pageX
          this.lastY = event.touches[0].pageY
      },
      handleTouchEnd(event) {
          this.flag = 0
      }
		}
	}
</script>

<style>
.bottle {
  width: 120rpx;
  height: 120rpx;
  background: url(../static/bottle.png) no-repeat;
  background-size: 100%;
}
.bottle-neck{
  width: 100%;
  height: 33rpx;
}
.bottle-body {
  width: 68rpx;
  height: 76rpx;
  margin-left: 27rpx;
}
.bottle-empty {
  width: 100%;
}
.bottle-full {
  width: 100%;
  background-color: #fbb1be;
  border-radius: 13rpx;
}
</style>
