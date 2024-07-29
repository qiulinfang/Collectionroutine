<template>
  <view v-if="showCountdown" class="countdown">
    <view>{{ days }} 天 {{ hours }} 时 {{ minutes }} 分 {{ seconds }} 秒</view>
  </view>
</template>

<script>
export default {
  name: 'Countdown',
  props: {
    targetTimestamp: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      countdown: null,
      showCountdown: false,
      days: '00',
      hours: '00',
      minutes: '00',
      seconds: '00',
    };
  },
  methods: {
    startCountdown() {
      this.showCountdown = true;
      this.countdown = setInterval(() => {
        const now = Date.now();
        const diff = this.targetTimestamp - now;

        if (diff <= 0) {
          clearInterval(this.countdown);
          this.showCountdown = false;
          this.days = '00';
          this.hours = '00';
          this.minutes = '00';
          this.seconds = '00';
          return;
        }

        const days = Math.floor(diff / (1000 * 60 * 60 * 24));
        const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
        const seconds = Math.floor((diff % (1000 * 60)) / 1000);

        this.days = ('0' + days).slice(-2);
        this.hours = ('0' + hours).slice(-2);
        this.minutes = ('0' + minutes).slice(-2);
        this.seconds = ('0' + seconds).slice(-2);
      }, 1000);
    },
    clearTimer() {
      clearInterval(this.countdown);
    }
  },
  mounted() {
    this.startCountdown();
  },
  beforeDestroy() {
    this.clearTimer();
  }
};
</script>

<style scoped>
.countdown {
  font-size: 16px;
}
</style>