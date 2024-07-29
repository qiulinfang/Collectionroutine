<template>
  <transition name="drawer-fade">
    <div
      v-if="visible"
      class="drawer"
    >
      <div class="drawer__content">
        <slot name="header">aaaaaaaaaa</slot>
        <slot></slot>
        <slot name="footer">aaaaaaaaaaaaaaaa</slot>
      </div>
      <button @click="toggle" class="drawer__close-button">Close</button>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Drawer',
  props: {
    visible: {
      type: Boolean,
      default: false
    },
  },
  methods: {
    toggle() {
      this.$emit('update');
    },
  }
}
</script>

<style scoped>
.drawer {
  position: fixed;
  bottom: 0;
	left:0;
	right: 0;
	height: 400rpx;
  background-color: white;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}


.drawer__content {
  padding: 16px;
}

.drawer__close-button {
  position: absolute;
  top: 10px;
  right: 10px;
}

.drawer-fade-enter-active,
.drawer-fade-leave-active {
  transition: opacity 0.3s;
}
.drawer-fade-enter-from,
.drawer-fade-leave-to {
  opacity: 0;
}

.is-visible {
  display: block;
}
</style>