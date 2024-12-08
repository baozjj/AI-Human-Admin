<script lang="ts" setup>
import {} from 'vue';
</script>

<template>
  <div class="iframe-container">
    <iframe class="responsive-iframe" src="http://127.0.0.1:5500/index.html" allow="microphone;"></iframe>
  </div>
</template>

<style scoped lang="scss">
.iframe-container {
  width: 100%;
  height: 100%;
  border-radius: 20px;
  overflow: hidden;
}

.responsive-iframe {
  width: 100%;
  height: 100%;
}
</style>
