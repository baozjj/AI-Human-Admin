<script setup lang="ts">
import { $t } from '@/locales';

defineOptions({
  name: 'RoleSearch'
});

interface Emits {
  (e: 'reset'): void;
  (e: 'search'): void;
}

const emit = defineEmits<Emits>();

// const model = defineModel<Api.SystemManage.RoleSearchParams>('model', { required: true });

const model = {
  roleName: '',
  roleCode: ''
};

function reset() {
  emit('reset');
}

function search() {
  emit('search');
}
</script>

<template>
  <ACard title="实体搜索" :bordered="false" class="card-wrapper">
    <AForm :model="model" :label-width="80">
      <ARow :gutter="[16, 16]" wrap>
        <ACol :span="24" :md="12" :lg="6">
          <AFormItem label="实体名称" name="roleName" class="m-0">
            <AInput v-model:value="model.roleName" :placeholder="$t('page.manage.role.form.roleName')" />
          </AFormItem>
        </ACol>
        <ACol :span="24" :md="12" :lg="6">
          <AFormItem label="实体类型" name="roleName" class="m-0">
            <AInput v-model:value="model.roleName" :placeholder="$t('page.manage.role.form.roleName')" />
          </AFormItem>
        </ACol>
        <ACol :span="24" :md="12" :lg="6">
          <AFormItem label="状态" name="roleCode" class="m-0">
            <AInput v-model:value="model.roleCode" :placeholder="$t('page.manage.role.form.roleCode')" />
          </AFormItem>
        </ACol>
        <ACol :span="24" :md="12" :lg="6">
          <AFormItem class="m-0">
            <div class="w-full flex-y-center justify-end gap-12px">
              <AButton @click="reset">
                <template #icon>
                  <icon-ic-round-refresh class="align-sub text-icon" />
                </template>
                <span class="ml-8px">{{ $t('common.reset') }}</span>
              </AButton>
              <AButton type="primary" ghost @click="search">
                <template #icon>
                  <icon-ic-round-search class="align-sub text-icon" />
                </template>
                <span class="ml-8px">{{ $t('common.search') }}</span>
              </AButton>
            </div>
          </AFormItem>
        </ACol>
      </ARow>
    </AForm>
  </ACard>
</template>

<style scoped></style>
