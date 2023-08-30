<template>
  <div class="as-tabs-pane">
    <!-- <transition :name="paneTransitionName"> -->
      <div class="tab-pane-content" v-show="$parent.activeName == name">
        <slot name="default"></slot>
      </div>
    <!-- </transition> -->
  </div>
</template>
  
<script>
export default {
  props: {
    // tab项的文本或者render函数
    label: {
      type: [String, Function],
    },
    // 每项标识
    name: {
      type: String,
    },
    // 是否禁用当前项
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      paneTransitionName: "enter-right",
    };
  },
  created() {
    // 统一tab的数据给父组件进行处理和渲染
    this.$parent.setTabBar(this);
  },
};
</script>
  
<style lang="less" scoped>
.as-tabs-pane {
  overflow-x: hidden;

  .enter-right-enter-active {
    transition: transform 0.35s;
  }

  .enter-right-enter {
    transform: translateX(100%);
  }

  .enter-right-to {
    transform: translateX(0);
  }
}
</style>
  
