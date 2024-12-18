<script setup lang="ts">
import { watch } from 'vue';
import { useAppStore } from '@/store/modules/app';
import { useEcharts } from '@/hooks/common/echarts';

defineOptions({
  name: 'TotalVisitTrendChart'
});

const appStore = useAppStore();

const { domRef, updateOptions } = useEcharts(() => ({
  tooltip: {
    trigger: 'axis',
    axisPointer: {
      type: 'cross',
      label: {
        backgroundColor: '#6a7985'
      }
    }
  },
  legend: {
    data: ['访问量']
  },
  grid: {
    left: '3%',
    right: '4%',
    bottom: '3%',
    containLabel: true
  },
  xAxis: {
    type: 'category',
    boundaryGap: false,
    data: [] as string[] // 时间轴数据（天数）
  },
  yAxis: {
    type: 'value'
  },
  series: [
    {
      color: '#5da8ff',
      name: '访问量',
      type: 'line',
      smooth: true,
      areaStyle: {
        color: {
          type: 'linear',
          x: 0,
          y: 0,
          x2: 0,
          y2: 1,
          colorStops: [
            { offset: 0.25, color: '#5da8ff' },
            { offset: 1, color: '#fff' }
          ]
        }
      },
      emphasis: { focus: 'series' },
      data: [] as number[] // 总访问量数据
    }
  ]
}));

// 模拟多天数据
async function mockData() {
  await new Promise(resolve => setTimeout(resolve, 1000));

  updateOptions(opts => {
    opts.xAxis.data = [
      '2024-06-10',
      '2024-06-11',
      '2024-06-12',
      '2024-06-13',
      '2024-06-14',
      '2024-06-15',
      '2024-06-16',
      '2024-06-17',
      '2024-06-18',
      '2024-06-19',
      '2024-06-20',
      '2024-06-21',
      '2024-06-22',
      '2024-06-23',
      '2024-06-24'
    ];
    opts.series[0].data = [1545, 1850, 2315, 1800, 2567, 2200, 3130, 2697, 2861, 3202, 3324, 3102, 3530, 3800, 3702];

    return opts;
  });
}

function updateLocale() {
  updateOptions((opts, factory) => {
    const originOpts = factory();
    opts.legend.data = ['访问量'];
    opts.series[0].name = '访问量';
    return opts;
  });
}

async function init() {
  mockData();
}

watch(
  () => appStore.locale,
  () => updateLocale()
);

init();
</script>

<template>
  <ACard :bordered="false" class="card-wrapper">
    <div ref="domRef" class="h-460px overflow-hidden"></div>
  </ACard>
</template>

<style scoped></style>
