<script setup lang="tsx">
import { onBeforeMount, ref } from 'vue';
import { Button, Popconfirm, Tag } from 'ant-design-vue';
import { fetchGetUserList } from '@/service/api';
import { useTable, useTableOperate, useTableScroll } from '@/hooks/common/table';
import { $t } from '@/locales';
import { enableStatusRecord } from '@/constants/business';
import UserOperateDrawer from './modules/user-operate-drawer.vue';
import UserSearch from './modules/user-search.vue';
import { mockData } from './service/const';
const { tableWrapperRef, scrollConfig } = useTableScroll();

const {
  columns,
  columnChecks,
  data,
  getData,
  getDataByPage,
  loading,
  mobilePagination,
  searchParams,
  resetSearchParams
} = useTable({
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
  columns: () => [
    {
      key: 'index',
      title: '序号',
      dataIndex: 'index',
      align: 'center',
      width: 64
    },
    {
      key: 'userName',
      dataIndex: 'userName',
      title: '头实体',
      align: 'center',
      minWidth: 100
    },
    {
      key: 'nickName',
      dataIndex: 'nickName',
      title: '关系',
      align: 'center',
      minWidth: 200
    },
    {
      key: 'updateBy',
      dataIndex: 'updateBy',
      title: '尾实体',
      align: 'center',
      minWidth: 100
    },
    {
      key: 'userPhone',
      dataIndex: 'userPhone',
      title: '创建时间',
      align: 'center',
      minWidth: 200
    },
    {
      key: 'userEmail',
      dataIndex: 'userEmail',
      title: '更新时间',
      align: 'center',
      minWidth: 200
    },
    {
      key: 'status',
      dataIndex: 'status',
      title: '状态',
      align: 'center',
      width: 100,
      customRender: ({ record }) => {
        if (record.status === null) {
          return null;
        }

        const tagMap: Record<Api.Common.EnableStatus, string> = {
          1: 'success',
          2: 'warning'
        };

        const label = $t(enableStatusRecord[record.status]);

        return <Tag color={tagMap[record.status]}>{label}</Tag>;
      }
    },
    {
      key: 'operate',
      title: $t('common.operate'),
      align: 'center',
      width: 180,
      customRender: ({ record }) => (
        <div class="flex-center gap-8px">
          <Button type="primary" ghost size="small" onClick={() => edit(record.id)}>
            {$t('common.edit')}
          </Button>
          <Button size="small" onClick={() => edit(record.id)}>
            详情
          </Button>
          <Popconfirm title={$t('common.confirmDelete')} onConfirm={() => handleDelete(record.id)}>
            <Button danger size="small">
              {$t('common.delete')}
            </Button>
          </Popconfirm>
        </div>
      )
    }
  ]
});

const {
  drawerVisible,
  operateType,
  editingData,
  handleAdd,
  handleEdit,
  checkedRowKeys,
  rowSelection,
  onBatchDeleted,
  onDeleted
  // closeDrawer
} = useTableOperate(data, getData);

async function handleBatchDelete() {
  // request
  // console.log(checkedRowKeys.value);

  onBatchDeleted();
}

function handleDelete(id: number) {
  // request
  console.log(id);

  onDeleted();
}

function edit(id: number) {
  handleEdit(id);
}

const pagination = ref({});

const mySearch = async () => {
  await getDataByPage();
  data.value = mockData;
  console.log(data.value);
  mobilePagination.value.total = 2000;
  pagination.value = mobilePagination.value;
  pagination.value.total = 3782;
};

onBeforeMount(() => {
  mySearch();
});
</script>

<template>
  <div class="min-h-500px flex-col-stretch gap-16px overflow-hidden lt-sm:overflow-auto">
    <UserSearch v-model:model="searchParams" @reset="resetSearchParams" @search="mySearch" />
    <ACard
      title="关系列表"
      :bordered="false"
      :body-style="{ flex: 1, overflow: 'hidden' }"
      class="flex-col-stretch sm:flex-1-hidden card-wrapper"
    >
      <template #extra>
        <TableHeaderOperation
          v-model:columns="columnChecks"
          :disabled-delete="checkedRowKeys.length === 0"
          :loading="loading"
          @add="handleAdd"
          @delete="handleBatchDelete"
          @refresh="getData"
        />
      </template>
      <ATable
        ref="tableWrapperRef"
        :columns="columns"
        :data-source="data"
        size="small"
        :row-selection="rowSelection"
        :scroll="scrollConfig"
        :loading="loading"
        row-key="id"
        :pagination="pagination"
        class="h-full"
      />

      <UserOperateDrawer
        v-model:visible="drawerVisible"
        :operate-type="operateType"
        :row-data="editingData"
        @submitted="mySearch"
      />
    </ACard>
  </div>
</template>

<style scoped></style>
