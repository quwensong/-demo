<template>
  <div class="as-tab">
    <div class="as-tab-nav">
      <div v-for="(item, index) in tabNavList" @click.stop="handleTabNavClick(item, index)"
        :class="['tab-nav-item', item.name == activeName ? 'active' : '']" ref="tabNavItemRefs">
        <span v-if="item.text">{{ item.text }}</span>
      </div>

      <div class="as-tab-active" :style="{ 'left': left, 'width': `${width}px` }">{{ activeName }}</div>

    </div>


    <div class="tab-content-wrap">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // v-model的那项
    value: {
      type: String,
    },
    // 是否显示滑块背景
    showTrackBg: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      // tab数组
      tabNavList: [],
      // 当前活跃项
      activeName: "",
      // 当前活跃索引
      currentIndex: 0,
      left: 0,
      width: 0
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    // 初始化
    init() {
      // 默认当前活跃项为外部v-model的值
      console.log(this.value)
      this.activeName = this.value;
      // 页面渲染任务之后计算滑块偏移量和宽度
      this.$nextTick(() => {
        this.currentIndex = this.$children.findIndex(
          (component) => component.name == this.value
        );
            this.headHeight();
      });

      window.onresize = this.debounce(() => {
        return ( () => {
          this.$nextTick(() => {
          //  this.headHeight();
          this.headHeight()
          });
        })();
      });
    },
    debounce(fn, delay=50) {
      // 1.定义一个定时器, 保存上一次的定时器
      let timer = null

      // 2.真正执行的函数
      const _debounce = function () {
        // 取消上一次的定时器
        if (timer) clearTimeout(timer)
        // 延迟执行
        timer = setTimeout(() => {
          // 外部传入的真正要执行的函数
          fn()
        }, delay)
      }

      return _debounce
    },
    headHeight() {
      
      this.offsetWidth = this.width = this.$refs.tabNavItemRefs[0].offsetWidth
      this.left = this.currentIndex * this.offsetWidth + "px";

      console.log(this.left, this.$refs.tabNavItemRefs[0].offsetWidth)
    },
    // 设置tab点击栏
    setTabBar(tabsPaneInstance) {
      // tab的描述信息可以是字符串也可以是render函数
      const label = tabsPaneInstance.label;
      // 添加到数组项中，根据添加条件渲染
      this.tabNavList.push({
        text: label,
        name: tabsPaneInstance.name,
      });
    },
    handleTabNavClick(item, index) {
      console.log(item, index, this.$refs.tabNavItemRefs[0].offsetWidth)
      // this.offsetWidth = this.$refs.tabNavItemRefs[0].offsetWidth
      // this.nowPage = page;
      this.title = item.text;
      this.left = index * this.offsetWidth + "px";
      if (item.name == this.activeName) return;
      // 更新当前活跃项
      this.activeName = item.name;
      // 活跃项的索引
      this.currentIndex = index;
      // 计算滑块的偏移量和宽度
    },

  },
};
</script>

<style lang="less" scoped>
@tab-height: 52px;
@tab-bgcolor: #f3fafb;
@primary-color: #000000;

.as-tab {
  .as-tab-nav {
    display: flex;
    position: relative;
    border-radius: 12px 12px 0 0;
    background-color: @tab-bgcolor;

    .tab-nav-item {
      cursor: pointer;
      line-height: 2;
      // flex: 1;
      width: 200px;
      height: @tab-height;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 15px;
      color: @primary-color;
      font-weight: 600;
      position: relative;
    }

    .tab-nav-item.active {
      color: #f3fafb;
    }


    .as-tab-active {
      position: absolute;
      width: 200px;
      height: 100%;
      color: #ffffff;
      left: 0;
      top: 0;
      opacity: 1;
      background: #ffffff;
      color: #0ba4b9;
      font-weight: 600;

      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 12px 12px 0 0;
      transition: all 0.2s;
    }
  }

  .v-enter-active,
  .v-leave-active {
    transition: all 0.3s;
  }

  .v-enter-to,
  .v-leave {
    transform: translate(0);
  }

  .v-enter {
    transform: translateX(100%);
  }

  /*.v-leave-to{*/
  /*    transform: translate(-100%);*/
  /*}*/

}
</style>
