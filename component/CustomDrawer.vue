<!-- CustomDrawer.vue -->
<template>
  <view class="custom-drawer-wrapper" :class="{ open: isDrawerOpen }">
    <view class="page-content">
      <!-- 页面内容 -->
    </view>
    <view class="drawer-content">
      <!-- 抽屉内容 -->
    </view>
    <view class="mask" @tap="closeDrawer"></view>
  </view>
</template>

<script>
export default {
  // ...
}
</script>

<style scoped>
.custom-drawer-wrapper {
  height: 100%;
  position: relative;
}

.page-content {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  transition: transform 0.3s ease;
}

.open .page-content {
  transform: translateY(-100%);
}

.drawer-content {
  /* 抽屉样式 */
}

.mask {
  /* 遮罩层样式 */
}
</style>