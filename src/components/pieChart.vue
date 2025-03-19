<template>
  <!-- 图表容器 -->
  <div class="chart-container">
    <div class="chart-tltie">接口分布</div>
    <div ref="chartRef" class="chart"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, onBeforeUnmount, watch } from "vue";
import * as echarts from "echarts";

// 绑定图表 DOM 容器
const chartRef = ref(null);
// 存储 ECharts 实例，避免重复创建
let chartInstance = null;

// 环形图数据
const chartData = ref([
  { name: "PMS接口", value: 126, color: "#18afa9" },
  { name: "总部ECS", value: 126, color: "#35b0de" },
  { name: "自建ECS", value: 126, color: "#65cb6c" },
  { name: "运营监控", value: 126, color: "#dac219" },
]);

// 计算数据总数（用于图表中心显示）
// const total = ref(chartData.value.reduce((sum, item) => sum + item.value, 0));
const total = ref(265);

/**
 * 获取 ECharts 配置项
 * @returns {Object} 配置项
 */
const getOption = () => ({
  title: {
    // 设置总数居中显示
    text: `{a|总数}\n\n{b|${total.value}}`,
    left: "30%",
    top: "center",
    textStyle: {
      rich: {
        a: {
          fontSize: 20,
          fontWeight: "bold",
          color: "#333333",
          align: "center",
        }, // "总数" 文字样式
        b: {
          fontSize: 36,
          fontWeight: "700",
          color: "#333",
          align: "center",
        }, // 总数数值样式
      },
    },
  },
  legend: {
    // 图例位置（右侧纵向排列）
    orient: "vertical",
    right: "100px",
    top: "center",
    itemWidth: 14, // 控制图例方块的宽度
    itemHeight: 14, // 控制图例方块的高度
    itemGap: 20, // **增加图例间隔**
    // 自定义图例格式
    formatter: (name) => {
      const item = chartData.value.find((item) => item.name === name);
      const percentage = "25";

      // **定义颜色 key**
      const index = chartData.value.findIndex((i) => i.name === name);
      const valueColorKey = `value${index}`;
      const percentColorKey = `percent${index}`;

      return `{name|${name}} {${valueColorKey}|${item.value}} {${percentColorKey}|${percentage}%}`;
    },
    textStyle: {
      rich: {
        name: {
          width: 50,
          fontSize: 14,
          fontWeight: 700,
          color: "#333",
          padding: [0, 40, 0, 0],
        },
        // **数值颜色**
        value0: {
          fontSize: 14,
          fontWeight: 700,
          color: "#18afa9",
          padding: [0, 20, 0, 10],
        },
        value1: {
          fontSize: 14,
          fontWeight: 700,
          color: "#35b0de",
          padding: [0, 20, 0, 10],
        },
        value2: {
          fontSize: 14,
          fontWeight: 700,
          color: "#65cb6c",
          padding: [0, 20, 0, 10],
        },
        value3: {
          fontSize: 14,
          fontWeight: 700,
          color: "#dac219",
          padding: [0, 20, 0, 10],
        },
        // **百分比颜色**
        percent0: {
          fontSize: 14,
          fontWeight: 700,
          color: "#18afa9",
          padding: [0, 0, 0, 0],
        },
        percent1: {
          fontSize: 14,
          fontWeight: 700,
          color: "#35b0de",
          padding: [0, 0, 0, 0],
        },
        percent2: {
          fontSize: 14,
          fontWeight: 700,
          color: "#65cb6c",
          padding: [0, 20, 0, 0],
        },
        percent3: {
          fontSize: 14,
          fontWeight: 700,
          color: "#dac219",
          padding: [0, 20, 0, 0],
        },
      },
    },
    icon: "rect",
    itemStyle: {
      borderRadius: 0,
    },
    icon: "rect", // **将图例图标设为矩形**
    itemStyle: {
      borderRadius: 0, // **让图例变成方形**
    },
  },
  series: [
    {
      type: "pie",
      radius: ["50%", "70%"], // 设置环形结构
      center: ["35%", "50%"], // **让饼图整体左移**
      avoidLabelOverlap: false, // 避免标签重叠
      label: { show: false }, // 默认隐藏标签
      emphasis: {
        // 鼠标悬浮时显示标签
        label: {
          show: true,
          fontSize: 14,
          fontWeight: "bold",
          formatter: "{b}\n{c} ({d}%)",
        },
      },
      data: chartData.value.map((item) => ({
        name: item.name,
        value: item.value,
        itemStyle: {
          color: item.color, // 设置颜色
          borderWidth: 2, // **设置扇区之间的间隔宽度**
          borderColor: "#fff", // **白色边框，形成空隙效果**
        },
      })),
    },
  ],
});

/**
 * 初始化 ECharts 实例
 */
const initChart = () => {
  // 如果已有实例，则销毁，防止重复创建
  if (chartInstance) {
    chartInstance.dispose();
  }
  // 创建新的 ECharts 实例
  chartInstance = echarts.init(chartRef.value);
  chartInstance.setOption(getOption());

  // 监听窗口变化，自适应调整图表大小
  window.addEventListener("resize", resizeChart);
};

/**
 * 调整图表大小
 */
const resizeChart = () => {
  if (chartInstance) {
    chartInstance.resize();
  }
};

// 监听数据变化，自动更新图表
watch(
  chartData,
  () => {
    if (chartInstance) {
      // 重新计算总数
      total.value = chartData.value.reduce((sum, item) => sum + item.value, 0);
      // 更新图表数据
      chartInstance.setOption(getOption());
    }
  },
  { deep: true }
);

/**
 * 组件挂载时初始化图表
 */
onMounted(() => {
  initChart();
});

/**
 * 组件卸载时清理资源
 */
onBeforeUnmount(() => {
  if (chartInstance) {
    chartInstance.dispose(); // 释放 ECharts 资源，防止内存泄漏
  }
  window.removeEventListener("resize", resizeChart); // 取消监听窗口变化
});
</script>

<style scoped lang="scss">
/* 图表容器，确保自适应 */
.chart-container {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 2px solid #e6f0ef;
  border-radius: 4px;
  position: relative;
  .chart-tltie {
    position: absolute;
    top: 0;
    left: 0;
    font-weight: 700;
    color: #333333;
    font-size: 20px;
    margin: 16px;
  }

  /* ECharts 画布 */
  .chart {
    width: 100%;
    min-height: 300px; /* 设置最小高度，防止内容塌陷 */
  }
}
</style>
