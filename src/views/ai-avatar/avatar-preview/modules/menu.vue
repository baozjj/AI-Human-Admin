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
  <div class="image-container">
    <img src="@/assets/imgs/avatar-bg/bg1.png" alt="Image 1" />
    <img src="@/assets/imgs/avatar-bg/bg2.png" alt="Image 1" />
    <img src="@/assets/imgs/avatar-bg/bg3.png" alt="Image 1" />
    <img src="@/assets/imgs/avatar-bg/bg4.png" alt="Image 1" />
  </div>
</template>

<style lang="scss" scoped>
:deep(.ant-menu-item) {
  height: 100px;
}

.image-container {
  display: flex;
  flex-direction: column; /* 竖直排列图片 */
  align-items: center; /* 图片水平居中 */
  justify-content: start;
  background-color: #fff;
  gap: 30px; /* 图片之间的间距 */
  height: 100%;
  width: 300px;
  border-radius: 0 10px 10px 0;
  padding: 20px;
}

.image-container img {
  width: 230px; /* 固定宽度 */
  height: 130px; /* 固定高度，确保图片一致 */
  object-fit: cover; /* 确保图片裁剪以填充大小 */
  border: 1px solid #ccc; /* 可选，添加边框 */
  border-radius: 10px;
}
</style>
