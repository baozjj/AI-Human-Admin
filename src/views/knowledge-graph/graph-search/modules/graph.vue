<script>
import * as echarts from 'echarts';

export default {
  name: 'CommunityKnowledgeGraph',
  mounted() {
    this.initChart();
  },
  methods: {
    initChart() {
      const chart = echarts.init(document.getElementById('chart-container'));

      // 数据结构调整：物业服务知识图谱
      const categories = [
        { name: '服务', color: 'rgba(0, 136, 184, 1)' }, // 服务
        { name: '楼栋', color: 'rgba(7, 214, 205, 1)' }, // 楼栋
        { name: '规章制度', color: 'rgba(236, 153, 41, 1)' }, // 规章制度
        { name: '活动', color: 'rgba(210, 142, 200, 1)' }, // 活动
        { name: '设施', color: 'rgba(255, 99, 132, 1)' }, // 设施
        { name: '服务方', color: 'rgba(153, 102, 255, 1)' } // 服务方
      ];

      // 真实数据：物业服务图谱
      const nodes = [
        {
          id: '6',
          name: '美好家园家政服务公司',
          category: 5,
          symbolSize: 80, // 中心节点更大
          attributes: [
            '公司名称: 美好家园家政服务公司',
            '服务类型: 家政服务',
            '联系方式: 139-xxxx-5678',
            '服务范围: 五星田里小区及周边区域',
            '服务提供者类型: 公司'
          ]
        },
        {
          id: '1',
          name: '房屋深度清洁',
          category: 0,
          symbolSize: 60,
          attributes: [
            '服务类型: 家政服务',
            '服务内容: 300元/次，包含厨房、卫生间、卧室等全面清洁',
            '联系方式: 173-xxxx-7890',
            '服务提供者: 美好家园家政服务公司'
          ]
        },
        {
          id: '7',
          name: '定期清洁',
          category: 0,
          symbolSize: 55,
          attributes: [
            '服务类型: 家政服务',
            '服务内容: 每月定期清洁一次，费用150元',
            '服务提供者: 美好家园家政服务公司'
          ]
        },
        {
          id: '8',
          name: '紧急维修',
          category: 0,
          symbolSize: 55,
          attributes: [
            '服务类型: 家政服务',
            '服务内容: 提供水电管道紧急维修服务，费用根据实际情况定',
            '服务提供者: 美好家园家政服务公司'
          ]
        },
        {
          id: '9',
          name: '物业管理办公室',
          category: 5,
          symbolSize: 60,
          attributes: ['办公室名称: 物业管理办公室', '负责人: 王经理', '联系方式: 189-xxxx-1122']
        },
        {
          id: '10',
          name: '维修人员',
          category: 5,
          symbolSize: 50,
          attributes: ['工作人员: 张大伟', '职位: 水电维修工程师', '联系方式: 188-xxxx-3344']
        },
        {
          id: '11',
          name: '小区居民',
          category: 5,
          symbolSize: 50,
          attributes: ['居民姓名: 李芳', '住址: 五星田里小区', '联系方式: 130-xxxx-5678']
        },
        {
          id: '12',
          name: '五星田里小区',
          category: 1,
          symbolSize: 60,
          attributes: ['小区名称: 五星田里小区', '楼栋数量: 20栋', '总住户: 500户', '位置: 市中心']
        },
        {
          id: '13',
          name: '清洁设备',
          category: 4,
          symbolSize: 55,
          attributes: ['设备名称: 高压蒸汽清洁机', '设备类型: 清洁设备', '使用年限: 2年', '维护人员: 李工']
        },
        {
          id: '14',
          name: '家电维修活动',
          category: 3,
          symbolSize: 55,
          attributes: [
            '活动名称: 迎新春社区联欢会',
            '活动类型: 社交、文艺',
            '活动时间: 2025年2月10日',
            '活动地点: 小区多功能厅'
          ]
        },
        {
          id: '15',
          name: '合作供应商',
          category: 5,
          symbolSize: 55,
          attributes: ['供应商名称: 星星维修公司', '服务类型: 水电维修', '联系方式: 136-xxxx-9876']
        }
      ];

      const links = [
        { source: '6', target: '1', label: { show: true, formatter: '提供' } }, // 美好家园家政服务公司提供房屋深度清洁
        { source: '6', target: '7', label: { show: true, formatter: '提供' } }, // 美好家园家政服务公司提供定期清洁
        { source: '6', target: '8', label: { show: true, formatter: '提供' } }, // 美好家园家政服务公司提供紧急维修
        { source: '6', target: '9', label: { show: true, formatter: '协作' } }, // 美好家园家政服务公司与物业管理合作
        { source: '6', target: '10', label: { show: true, formatter: '合作' } }, // 美好家园家政服务公司与维修人员合作
        { source: '6', target: '11', label: { show: true, formatter: '服务于' } }, // 美好家园家政服务公司服务于小区居民
        { source: '6', target: '12', label: { show: true, formatter: '提供服务给' } }, // 美好家园家政服务公司提供服务给五星田里小区
        { source: '6', target: '13', label: { show: true, formatter: '使用' } }, // 美好家园家政服务公司使用清洁设备
        { source: '6', target: '14', label: { show: true, formatter: '组织' } }, // 美好家园家政服务公司组织社区活动
        { source: '6', target: '15', label: { show: true, formatter: '合作' } }, // 美好家园家政服务公司与合作供应商合作
        { source: '1', target: '13', label: { show: true, formatter: '使用' } }, // 房屋深度清洁使用清洁设备
        { source: '12', target: '14', label: { show: true, formatter: '举办' } }, // 五星田里小区举办迎新春社区联欢会
        { source: '10', target: '15', label: { show: true, formatter: '外包服务' } } // 维修人员外包服务给星星维修公司
      ];

      const option = {
        tooltip: {
          formatter: params => {
            if (params.dataType === 'node') {
              const node = params.data;
              let tooltipContent = `节点: ${node.name}`;
              if (node.attributes.length > 0) {
                tooltipContent += `<br/>属性: <ul>${node.attributes.map(attr => `<li>${attr}</li>`).join('')}</ul>`;
              }
              return tooltipContent;
            } else if (params.dataType === 'edge') {
              return `关系: ${params.data.label?.formatter || ''}`;
            }
          }
        },
        legend: [
          {
            data: categories.map(c => c.name),
            orient: 'vertical',
            left: 'right'
          }
        ],
        series: [
          {
            type: 'graph',
            layout: 'force',
            data: nodes,
            links,
            categories,
            roam: true,
            label: {
              show: true,
              position: 'right',
              formatter: '{b}'
            },
            lineStyle: {
              color: 'source',
              curveness: 0.3
            },
            force: {
              repulsion: 1000,
              edgeLength: [50, 200]
            }
          }
        ]
      };

      chart.setOption(option);
      window.addEventListener('resize', chart.resize);
    }
  }
};
</script>

<template>
  <div id="chart-container" class="chart-container"></div>
</template>

<style>
.chart-container {
  width: 100%;
  height: 600px;
}
</style>
