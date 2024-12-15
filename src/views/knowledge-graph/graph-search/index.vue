<script setup lang="tsx">
import { onBeforeMount } from 'vue';
import { fetchGetUserList } from '@/service/api';
import { useTable } from '@/hooks/common/table';
import UserSearch from './modules/user-search.vue';

import Graph from './modules/graph.vue';

const { searchParams, resetSearchParams } = useTable({
  apiFn: fetchGetUserList,
  apiParams: {
    current: 1,
    size: 10,
    // if you want to use the searchParams in Form, you need to define the following properties, and the value is null
    // the value can not be undefined, otherwise the property in Form will not be reactive
    status: undefined,
    userName: undefined,
    userGender: undefined,
    nickName: undefined,
    userPhone: undefined,
    userEmail: undefined
  },
  columns: () => []
});

const mySearch = async () => {};

onBeforeMount(() => {
  mySearch();
});
</script>

<template>
  <div class="min-h-500px flex-col-stretch gap-16px overflow-hidden lt-sm:overflow-auto">
    <UserSearch v-model:model="searchParams" @reset="resetSearchParams" @search="mySearch" />
    <ACard
      title="图谱"
      :bordered="false"
      :body-style="{ flex: 1, overflow: 'hidden' }"
      class="flex-col-stretch sm:flex-1-hidden card-wrapper"
    >
      <Graph></Graph>
    </ACard>
  </div>
</template>

<style scoped></style>
