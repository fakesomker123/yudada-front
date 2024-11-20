<template>
  <div id="appStatisticPage">
    <!-- 热门应用统计标题 -->
    <h2 class="page-title">热门应用统计</h2>
    <!-- 热门应用统计图表 -->
    <v-chart :option="appAnswerCountOptions" style="height: 300px" class="chart-style" />
    <!-- 应用结果统计标题 -->
    <h2 class="page-title">应用结果统计</h2>
    <!-- 搜索框部分 -->
    <div class="search-bar-wrapper">
      <a-input-search
        :style="{ width: '320px' }"
        placeholder="输入 appId（可手动输入其他id查看对应数据）"
        button-text="搜索"
        size="large"
        search-button
        @search="(value) => loadAppAnswerResultCountData(value)"
      />
    </div>
    <div style="margin-bottom: 16px" />
    <!-- 应用结果统计图表 -->
    <v-chart :option="appAnswerResultCountOptions" style="height: 300px" class="chart-style" />
  </div>
</template>

<script setup lang="ts">
import { computed, ref, onMounted, watchEffect } from "vue";
import API from "@/api";
import message from "@arco-design/web-vue/es/message";
import {
  getAppAnswerCountUsingGet,
  getAppAnswerResultCountUsingGet,
} from "@/api/appStatisticController";
import VChart from "vue-echarts";
import "echarts";

// 存储热门应用统计数据列表
const appAnswerCountList = ref<API.AppAnswerCountDTO[]>([]);
// 存储应用结果统计数据列表
const appAnswerResultCountList = ref<API.AppAnswerCountDTO[]>([]);

// 页面初始化时加载热门应用统计数据
const initAppAnswerCountData = async () => {
  try {
    const res = await getAppAnswerCountUsingGet();
    if (res.data.code === 0) {
      appAnswerCountList.value = res.data.data || [];
    } else {
      message.error("初始化热门应用统计数据失败，" + res.data.message);
    }
  } catch (error) {
    message.error("初始化热门应用统计数据出现异常，请稍后重试");
  }
};

// 页面初始化时加载应用结果统计数据（仅加载appId为3的数据）
const initAppAnswerResultCountData = async () => {
  try {
    const appId = 3; // 定义要获取数据的应用id为3
    const res = await getAppAnswerResultCountUsingGet({ appId });
    if (res.data.code === 0) {
      appAnswerResultCountList.value = res.data.data || [];
    } else {
      message.error("初始化应用结果统计数据失败，" + res.data.message);
    }
  } catch (error) {
    message.error("初始化应用结果统计数据出现异常，请稍后重试");
  }
};

// 加载数据（原用于根据输入id搜索结果统计数据，保留功能）
const loadAppAnswerResultCountData = async (appId: string) => {
  if (!appId) {
    return;
  }
  try {
    const res = await getAppAnswerResultCountUsingGet({ appId: appId as any });
    if (res.data.code === 0) {
      appAnswerResultCountList.value = res.data.data || [];
    } else {
      message.error("获取应用结果统计数据失败，" + res.data.message);
    }
  } catch (error) {
    message.error("搜索应用结果统计数据出现异常，请稍后重试");
  }
};

// 热门应用统计选项（更新数据获取逻辑）
const appAnswerCountOptions = computed(() => {
  return {
    xAxis: {
      type: "category",
      data: appAnswerCountList.value.map((item) => item.appId),
      name: "应用 id",
    },
    yAxis: {
      type: "value",
      name: "用户答案数",
    },
    series: [
      {
        data: appAnswerCountList.value.map((item) => item.answerCount),
        type: "bar",
      },
    ],
  };
});

// 应用结果统计选项（更新数据获取逻辑）
const appAnswerResultCountOptions = computed(() => {
  return {
    tooltip: {
      trigger: "item",
    },
    legend: {
      orient: "vertical",
      left: "left",
    },
    series: [
      {
        name: "应用答案结果分布",
        type: "pie",
        radius: "50%",
        data: appAnswerResultCountList.value.map((item) => {
          return { value: item.resultCount, name: item.resultName };
        }),
        emphasis: {
          itemStyle: {
            shadowBlur: 10,
            shadowOffsetX: 0,
            shadowColor: "rgba(0, 0, 0, 0.5)",
          },
        },
      },
    ],
  };
});

// 页面加载时触发初始化数据加载
onMounted(() => {
    initAppAnswerCountData();
    initAppAnswerResultCountData();
});

// 监听相关变量，若有变化可按需重新加载数据（目前保留原逻辑，可根据实际需求细化）
watchEffect(() => {
  initAppAnswerCountData();
});

watchEffect(() => {
  initAppAnswerResultCountData();
});

watchEffect(() => {
  loadAppAnswerResultCountData("");
});
</script>

<style scoped>
.page-title {
  font-size: 24px;
  color: #795548; /* 设置标题文字颜色为深棕色，与暖色调主题适配，突出显示 */
  margin-bottom: 16px;
}

.chart-style {
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* 为图表添加阴影效果，使其更立体突出 */
  margin-bottom: 20px;
  background-color: white; /* 设置图表背景为白色，内容更清晰 */
}

.search-bar-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

a-input-search {
  border: 1px solid #BDBDBD; /* 搜索框边框颜色设为银灰色，更显柔和 */
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 10px;
  background-color: #FFE0B2; /* 搜索框主体背景设为淡橙色，与整体暖色调风格统一 */
}
</style>