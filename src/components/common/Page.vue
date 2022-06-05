<template>
  <mescroll-body
    @init="customInit"
    @up="$emit('up', $event)"
    @down="$emit('down', $event)"
    :fixed="false"
    :up="upOpts"
    :down="downOpts"
  >
    <slot />
  </mescroll-body>
</template>

<script>
import MescrollBody from "mescroll-uni/mescroll-body.vue";
export default {
  components: { MescrollBody },
  props: {
    up: {
      type: Boolean,
      default: true,
    },
    down: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      downOpts: {
        use: this.down, // 是否启用下拉刷新; 默认true
        auto: false, // 是否在初始化完毕之后自动执行下拉刷新的回调; 默认true
        autoShowLoading: false, // 如果设置auto=true(在初始化完毕之后自动执行下拉刷新的回调),那么是否显示下拉刷新的进度; 默认false
        isLock: false, // 是否锁定下拉刷新,默认false;
        offset: 80, // 在列表顶部,下拉大于80upx,松手即可触发下拉刷新的回调
        bottomOffset: 80, // 当手指touchmove位置在距离body底部20upx范围内的时候结束上拉刷新,避免Webview嵌套导致touchend事件不执行
        minAngle: 45, // 向下滑动最少偏移的角度,取值区间  [0,90];默认45度,即向下滑动的角度大于45度则触发下拉;而小于45度,将不触发下拉,避免与左右滑动的轮播等组件冲突;
        beforeEndDelay: 100, // 延时结束的时长 (显示加载成功/失败的时长, android小程序设置此项结束下拉会卡顿, 配置后请注意测试)
        bgColor: "#fff", // 背景颜色 (建议在pages.json中再设置一下backgroundColorTop)
        textColor: "#999", // 文本颜色 (当bgColor配置了颜色,而textColor未配置时,则textColor会默认为白色)
        textInOffset: "下拉刷新", // 下拉的距离在offset范围内的提示文本
        textOutOffset: "释放更新", // 下拉的距离大于offset范围的提示文本
        textLoading: "加载中 ...", // 加载中的提示文本
      },
      upOpts: {
        use: this.up, // 是否启用上拉加载; 默认true
        auto: false, // 是否在初始化完毕之后自动执行上拉加载的回调; 默认true
        isLock: false, // 是否锁定上拉加载,默认false;
        isBoth: false, // 上拉加载时,如果滑动到列表顶部是否可以同时触发下拉刷新;默认true,两者可同时触发;
        page: {
          num: 0, // 当前页码,默认0,回调之前会加1,即callback(page)会从1开始
          size: 10, // 每页数据的数量
        },
        noMoreSize: 3, // 如果列表已无数据,可设置列表的总数量要大于等于5条才显示无更多数据;避免列表数据过少(比如只有一条数据),显示无更多数据会不好看
        bgColor: "transparent", // 背景颜色
        textColor: "#999", // 文本颜色 (当bgColor配置了颜色,而textColor未配置时,则textColor会默认为白色)
        textLoading: "加载中 ...", // 加载中的提示文本
        textNoMore: "-- 没有更多了 --", // 没有更多数据的提示文本
        toTop: { src: "" },
        empty: { use: false },
      },
    };
  },
  methods: {
    customInit(entity) {
      entity.i18n = null; // 清掉i18n
      const payload = { name: this.$Route.name, value: entity };
      this.$store.commit("app/setMescrollByName", payload);
    },
  },
};
</script>