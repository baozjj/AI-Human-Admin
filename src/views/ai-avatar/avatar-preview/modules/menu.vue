<script lang="ts" setup>
import { h, reactive } from 'vue';
import { ControlFilled, HolderOutlined, MehOutlined, PicCenterOutlined, UserOutlined } from '@ant-design/icons-vue';
import type { ItemType, MenuMode, MenuTheme } from 'ant-design-vue';

const state = reactive({
  mode: 'inline' as MenuMode,
  theme: 'light' as MenuTheme,
  selectedKeys: ['1'],
  openKeys: ['sub1']
});

function getItem(label: string, key: string, icon?: any, children?: ItemType[], type?: 'group'): ItemType {
  return {
    key,
    icon,
    children,
    label,
    type
  } as ItemType;
}

const items: ItemType[] = reactive([
  getItem('数字人', '1', h(UserOutlined)),
  getItem('背景图', '2', h(PicCenterOutlined)),
  getItem('服饰搭配', '3', h(ControlFilled)),
  getItem('动作展示', '4', h(MehOutlined)),
  getItem('自定义', '5', h(HolderOutlined))
]);
</script>

<template>
  <AMenu
    v-model:open-keys="state.openKeys"
    v-model:selected-keys="state.selectedKeys"
    style="width: 136px"
    :mode="state.mode"
    :items="items"
    :theme="state.theme"
  ></AMenu>
  <div class="right"></div>
</template>

<style lang="scss" scoped>
:deep(.ant-menu-item) {
  height: 100px;
}

.right {
  background-color: #fff;
  height: 100%;
  width: 300px;
  border-radius: 0 10px 10px 0;
}
</style>
