<template>
  <div>
    <h1>scroller插件--移动端</h1>
    <div class="box">
      <div slot="demo" class="demo">
        <scroll ref="scroll"
                :data="analog_data"
                :scrollbar="scrollbarObj"
                :pullDownRefresh="pullDownRefreshObj"
                :pullUpLoad="pullUpLoadObj"
                :startY="parseInt(startY)"
                @pullingDown="onPullingDown"
                @pullingUp="onPullingUp">
          <div class="list-content">
            <div class="list-item" v-for="item in analog_data" v-text="item.text"></div>
          </div>
        </scroll>
      </div>
    </div>
  </div>
</template>
<script>
  import Scroll from '@/components/scroller/scroller.vue'

  export default {
    data() {
      return {
        analog_data: [],
        pulldown: true,
        scrollbar: true,
        scrollbarFade: true,
        pullDownRefresh: true,
        pullDownRefreshThreshold: 90,
        pullDownRefreshStop: 40,
        pullUpLoad: true,
        pullUpLoadThreshold: 0,
        pullUpLoadMoreTxt: '加载更多',
        pullUpLoadNoMoreTxt: '没有更多了',
        startY: 0,
        scrollToX: 0,
        scrollToY: -200,
        scrollToTime: 700,
        scrollToEasing: 'bounce',
        scrollToEasingOptions: ['bounce', 'swipe', 'swipeBounce'],

        //模拟数据index
        analog_index: 0

      }
    },
    created() {
      //初始数据 模拟请求
      setTimeout(() => {
        for (let i = 0; i < 5; i++) {
          this.analog_data.push({text: `This a analog data for ${i}`})
          this.analog_index = i;
        }
      }, 500)
    },
    mounted() {

    },
    watch: {
      scrollbarObj: {
        handler() {
          this.rebuildScroll()
        },
        deep: true
      },
      pullDownRefreshObj: {
        handler() {
          this.rebuildScroll()
        },
        deep: true
      },
      pullUpLoadObj: {
        handler() {
          this.rebuildScroll()
        },
        deep: true
      },
      startY() {
        this.rebuildScroll()
      }
    },
    computed: {
      scrollbarObj: function () {
        return this.scrollbar ? {fade: this.scrollbarFade} : false
      },
      pullDownRefreshObj: function () {
        return this.pullDownRefresh ? {
          threshold: parseInt(this.pullDownRefreshThreshold),
          stop: parseInt(this.pullDownRefreshStop)
        } : false
      },
      pullUpLoadObj: function () {
        return this.pullUpLoad ? {
          threshold: parseInt(this.pullUpLoadThreshold),
          txt: {more: this.pullUpLoadMoreTxt, noMore: this.pullUpLoadNoMoreTxt}
        } : false
      }
    },
    methods: {
      onPullingDown() {
        // 模拟更新数据
        console.log('pulling down and load data')
        setTimeout(() => {
          if (Math.random() > 0.5) {
            // 如果有新数据
            this.analog_data.unshift({text: 'newDataTxt:' + +new Date()})
          } else {
            // 如果没有新数据
            this.$refs.scroll.forceUpdate()
          }
        }, 2000)
      },
      onPullingUp() {
        // 更新数据
        console.log('pulling up and load data')
        setTimeout(() => {
          if (Math.random() > 0.5) {
            // 如果有新数据
            let newPage = []
            for (let i = 0; i < 10; i++) {
              newPage.push({text: `This a analog data for ${this.analog_index + i + 1}`})
            }
            this.analog_index += 10;
            this.analog_data = this.analog_data.concat(newPage)
          } else {
            // 如果没有新数据
            this.$refs.scroll.forceUpdate()
          }
        }, 1500)
      },
      rebuildScroll() {
        this.nextTick(() => {
          this.$refs.scroll.destroy()
          this.$refs.scroll.initScroll()
        })
      }
    },
    components: {
      scroll: Scroll
    }
  }
</script>
<style>
  .box {
    width: 375px;
    height: 667px;
    border: 1px solid #888;
    border-radius: 8px;
    position: relative;
    overflow: hidden;
    margin: 0 auto;
  }

  .demo {
    border-radius: 8px;
  }
</style>
