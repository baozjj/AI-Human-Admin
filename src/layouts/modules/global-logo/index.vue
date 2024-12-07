<script setup lang="ts">
import { $t } from '@/locales';

defineOptions({
  name: 'GlobalLogo'
});

interface Props {
  /** Whether to show the title */
  showTitle?: boolean;
}

withDefaults(defineProps<Props>(), {
  showTitle: true
});
</script>

<template>
  <RouterLink to="/" class="w-full flex-center nowrap-hidden">
    <!-- <SystemLogo class="tex/t-primary text-32px" /> -->
    <div class="logo-wrapper">
      <img src="@/assets/imgs/login_tubiao.png" class="logo" />
    </div>
    <h2 v-show="showTitle" class="pl-8px text-16px text-primary font-bold transition duration-300 ease-in-out">
      {{ $t('system.title') }}
    </h2>
  </RouterLink>
</template>

<style scoped>
.logo-wrapper {
  width: 40px;
  height: 40px;
  border-radius: 15px;
  background: linear-gradient(180deg, rgba(58, 157, 255, 1) 0%, rgba(0, 128, 255, 1) 100%);
  text-align: center;
  box-shadow: 0px 2px 30px 0px rgba(0, 128, 255, 0.16);
  display: flex;
  justify-content: center;
  align-items: center;

  .logo {
    width: 20px;
  }
}
</style>
