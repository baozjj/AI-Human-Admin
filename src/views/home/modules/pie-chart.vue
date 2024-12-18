<script setup lang="ts">
import { watch } from 'vue';
import { useAppStore } from '@/store/modules/app';
import { useEcharts } from '@/hooks/common/echarts';

defineOptions({
  name: 'DigitalHumanServiceChart'
});

const appStore = useAppStore();

// 使用ECharts的封装
const { domRef, updateOptions } = useEcharts(() => ({
  tooltip: {
    trigger: 'item',
    formatter: '{b} : {c} ({d}%)'
  },
  legend: {
    bottom: '1%',
    left: 'center',
    itemStyle: {
      borderWidth: 0
    }
  },
  series: [
    {
      color: ['#5da8ff', '#8e9dff', '#fedc69', '#26deca', '#ff7a7a'],
      name: '数字人服务请求统计',
      type: 'pie',
      radius: ['40%', '70%'],
      avoidLabelOverlap: false,
      itemStyle: {
        borderRadius: 10,
        borderColor: '#fff',
        borderWidth: 1
      },
      label: {
        show: true,
        position: 'outside',
        formatter: '{b}: {c}次 ({d}%)'
      },
      emphasis: {
        label: {
          show: true,
          fontSize: '14',
          fontWeight: 'bold'
        }
      },
      labelLine: {
        show: true
      },
      data: [] as { name: string; value: number }[]
    }
  ]
}));

// 模拟真实的数字人服务请求分类数据
async function mockServiceRequestData() {
  await new Promise(resolve => {
    setTimeout(resolve, 1000); // 模拟异步请求延迟
  });

  updateOptions(opts => {
    opts.series[0].data = [
      { name: '报修服务', value: 230 },
      { name: '费用查询', value: 180 },
      { name: '社区活动咨询', value: 120 },
      { name: '物业规章咨询', value: 95 },
      { name: '投诉反馈', value: 85 },
      { name: '设施预约', value: 110 },
      { name: '家政服务', value: 135 },
      { name: '停车位管理', value: 150 }
    ];
    return opts;
  });
}

// 监听语言变化并更新图表
function updateLocale() {
  updateOptions(opts => {
    opts.series[0].name = '数字人服务请求统计';
    return opts;
  });
}

// 初始化图表数据
async function init() {
  await mockServiceRequestData();
}

watch(
  () => appStore.locale,
  () => {
    updateLocale();
  }
);

// 执行初始化
init();
</script>

<!--
 <template>
  <div ref="domRef" style="width: 100%; height: 400px;"></div>
</template>

<style scoped>
/* 样式部分可根据实际需求自行修改 */
</style>
-->

<template>
  <ACard :bordered="false" class="card-wrapper">
    <div ref="domRef" class="h-460px overflow-hidden"></div>
  </ACard>
</template>

<style scoped></style>
