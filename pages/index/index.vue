<template>
	<view class="content" :style="{ background: backgroundColor}">
    <view v-if="showBottles">
      <view class="title">{{title}}</view>
      
      <view class="bollte-wrap">
        <view v-for="(item, index) in heartBottles" :key="index">
          <view class="bottle-item">
            <bottle v-if="hackReset"></bottle>
            <text class="text-info">{{item}}</text>
          </view>
        </view>
      </view>
      
      <view class="button-sp-area save-pictrue">
        <button class="mini-btn" type="primary" size="mini" open-type="share">分享</button>
        <button class="mini-btn change-gender" type="default" size="mini" @click="changeGender">切换</button>
      </view>
    </view>
    
    <view v-else>
      <view class="button-sp-area select-buttons">
        <button class="mini-btn" type="primary" size="mini" @click="handleGender('boy')">我是男生</button>
        <view class="select-buttons-diliver">or</view>
        <button class="mini-btn" type="warn" size="mini" @click="handleGender('gril')">我是女生</button>
      </view>
    </view>
	</view>
</template>

<script>
  import bottle from '@/components/bottle.vue'
  
	export default {
    components: {
      bottle
    },
		data() {
			return {
        hackReset: true,
				backgroundColor: '#fbb1be',
        showBottles: false,
        gender: null,
        bottles: {
          'boy': ['颜值高', '胸大', '手好看', '锁骨', 'Kiss', '牵手', '拥抱', '酷Girl', '情话', '细心', '御姐', '软妹', '弹钢琴', '腿好看', '活泼', '衣品好', '声音好听', '会唱歌', '马甲线', '温柔', '会做饭', '体贴', '气质好', '有耐心', '身材好', '二次元', '成熟', '可爱', '会拍照', '孝顺'],
          'gril': ['颜值高', '腹肌', '手好看', '锁骨', 'Kiss', '牵手', '拥抱', 'Wink', '情话', '细心', '爷们', '弹吉他', '弹钢琴', '打篮球', '踢足球', '衣品好', '声音好听', '会唱歌', '幽默', '有责任感', '会做饭', '体贴', '细心', '有耐心', '奶', '狼', '欲', '禁欲', '会拍照', '孝顺']
        }
      }
		},
    computed: {
      heartBottles() {
        if (!this.gender) {
          return []
        }
        return this.bottles[this.gender]
      },
      title() {
        if (this.gender == 'boy') {
          return "我的心动女生"
        }
         
        return "我的心动男生"
      }
    },
		onLoad() {

		},
		methods: {
      changeGender() {
        this.hackReset = false

        if (this.gender == 'boy') {
          this.gender = 'gril'
        } else {
          this.gender = 'boy'
        }
        
        this.$nextTick(() => {
          // 重载组件
          this.hackReset = true
        })
      },
      handleGender(type) {
        this.gender = type
        this.showBottles = true
        this.backgroundColor = '#fff'
      },
      share() {
        uni.share({
            provider: 'weixin',
            scene: "WXSceneSession",
            type: 5,
            title: '快来填满你的那个 Ta 吧！',
            miniProgram: {
                id: 'gh_abcdefg',
                path: '/pages/index/index',
                type: 0,
            },
            success: ret => {
                console.log(JSON.stringify(ret))
            }
        })
      }
		},
    onShareAppMessage(res) {
      if (res.from === 'button') {// 来自页面内分享按钮
        console.log(res.target)
      }
      return {
        title: '快来填满你的那个 Ta 吧！',
        path: '/pages/index/index'
      }
    }
	}
</script>

<style>
	.content {
    position: absolute;
    height: 100%;
    width: 100%;
    margin: 0;
    padding: 0;
		display: flex;
		flex-direction: column;
		align-items: center;
		/* justify-content: center; */
	}
  .title {
    margin: 30rpx;
    color: #fba4b4;
    text-align: center;
    font-weight: bold;
  }
  .bollte-wrap {
  	display: flex;
  	flex-wrap: wrap;
    justify-content: center;
  }
  .bottle-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 20rpx 0;
    width: 120rpx;
  }
  .text-info {
    padding: 0 10rpx;
    font-size: 12px;
  }
  .select-buttons {
    margin-top: 20rpx;
    margin-top: 400rpx;
    display: flex;
  }
  .select-buttons-diliver {
    margin: auto 20rpx;
  }
	.save-pictrue {
    text-align: center;
  }
  .change-gender {
    margin-left: 20rpx;
  }
</style>
