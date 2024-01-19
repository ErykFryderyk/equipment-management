<template>
  <div>
    <h2>Progres Bar</h2>
    <div class="progress-container">
      <div class="progress-bar" :style="{ width: progress + '%' }"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProgressBar',
  data() {
    return {
      progress: 0,
      intervalId: null,
    };
  },
  methods: {
    startProgressBar() {
      this.progress = 0;
      this.intervalId = setInterval(() => {
        this.progress += 10; // Zwiększ wartość o 10% co sekundę
        if (this.progress >= 100) {
          clearInterval(this.intervalId); // Zatrzymaj interwał po osiągnięciu 100%
        }
      }, 1000);
    },
  },
  mounted() {
    this.startProgressBar();
  },
};
</script>

<style scoped>
.progress-container {
  width: 100%;
  background-color: #f0f0f0;
  height: 20px;
  margin-top: 10px;
  overflow: hidden;
}

.progress-bar {
  height: 100%;
  background-color: #4caf50;
  transition: width 1s linear;
}
</style>
