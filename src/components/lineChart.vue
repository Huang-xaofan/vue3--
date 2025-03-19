<template>
  <div class="chart-container">
    <!-- 按钮切换 -->
    <div class="chart-tabs">
      <span
        ><el-button
          :type="activeTab === 'share' ? 'primary' : 'default'"
          @click="changeTab('share')"
          :style="{
            backgroundColor: activeTab === 'share' ? '#0f988d' : '#65CB6C',
            color: 'white',
          }"
        >
          共享数据
        </el-button></span
      >
      <span>
        <el-button
          :type="activeTab === 'request' ? 'primary' : 'default'"
          @click="changeTab('request')"
        >
          调用次数
        </el-button></span
      >
    </div>

    <!-- 图表 -->
    <div ref="chartRef" class="chart"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
import * as echarts from "echarts";

const chartRef = ref(null); // 绑定图表的 DOM
const chartInstance = ref(null); // 存储 ECharts 实例
const activeTab = ref("share"); // 当前选中的数据类型

// 数据集
const dataSets = {
  share: [68, 55, 40, 36, 40, 60, 80, 90, 85, 75, 72, 80], // 共享数据
  request: [100, 95, 90, 85, 80, 75, 70, 65, 60, 55, 50, 45], // 调用次数
};

// X 轴日期
const dateLabels = [
  "8-20",
  "8-21",
  "8-22",
  "8-23",
  "8-24",
  "8-25",
  "8-26",
  "8-27",
  "8-28",
  "8-29",
  "8-30",
  "9-01",
];

// 初始化图表
const initChart = () => {
  if (chartInstance.value) {
    chartInstance.value.dispose(); // 先销毁已有的实例，防止多次初始化
  }

  chartInstance.value = echarts.init(chartRef.value); // 创建实例

  const option = {
    grid: {
      left: "2%", // 左侧贴边
      right: "2%", // 右侧贴边
      top: "10%", // 上侧留一些空间
      bottom: "5%", // 底部留一些空间
      containLabel: true, // 避免标签被裁剪
    },
    xAxis: {
      type: "category",
      data: dateLabels, // X 轴数据
      axisLine: {
        show: false, //  显示 X 轴的主轴线
        lineStyle: {
          color: "#666", // x 轴线颜色
        },
      },
      axisTick: {
        show: false, // 隐藏 X 轴的刻度线
      },
      splitLine: {
        show: false, // 隐藏 X 轴的网格线
      },
      axisLabel: {
        color: "#333333", //设置 X 轴字体颜色
        fontSize: 12, // 字体大小
        fontWeight: "bold", //  字体加粗
      },
    },
    yAxis: {
      type: "value",
      name: "单位 ：条", // Y 轴单位
      nameLocation: "end", // 位置调整 ("start", "middle", "end")
      nameTextStyle: {
        padding: [0, 10, 0, 0], // 调整单位文本的间距
        fontSize: 12, // 文字大小
        color: "#666", // 文字颜色
      },
    },
    series: [
      {
        data: dataSets[activeTab.value], // 共享数据 or 调用次数
        type: "line",
        smooth: true, // 平滑曲线
        showSymbol: false, // 关闭数据点
        areaStyle: { color: "#0f988d", opacity: 0.08 }, // 区域背景
        lineStyle: { color: "#0f988d", width: 2 }, // 线条样式
      },
    ],
  };

  chartInstance.value.setOption(option); // 设置图表配置
};

// 监听 tab 切换，更新数据
watch(activeTab, () => {
  if (chartInstance.value) {
    chartInstance.value.setOption({
      series: [{ data: dataSets[activeTab.value] }],
    });
  }
});

// 组件挂载后初始化图表
onMounted(() => {
  initChart();
});

// 切换数据类型
const changeTab = (type) => {
  activeTab.value = type;
};
</script>

<style scoped lang="scss">
.chart-container {
  width: 100%;
  height: 100%;
  border: 2px solid #e6f0ef;
  border-radius: 4px;
  .chart-tabs {
    display: flex;
    justify-content: left;
    margin: 10px;
  }

  .chart {
    width: 100%;
    height: 300px;
  }
}
</style>
