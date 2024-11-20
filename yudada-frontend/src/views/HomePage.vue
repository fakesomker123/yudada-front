<template>
  <div id="homePage">
    <div class="page-header">
      <h1>灵思答语阁</h1>
      <p>灵思答语阁，邂逅文艺智慧，解锁答题新境</p>
    </div>
    <div class="search-bar-wrapper">
      <div class="search-bar">
        <a-input-search
          :style="{ width: '480px' }"
          placeholder="请输入关键词查找答题应用哦~ 这只是个摆设=-="
          button-text="查找"
          size="large"
          search-button
          @search="fetchAppData"
        />
      </div>
    </div>
    <div class="app-list-container">
      <a-list
        class="list-demo-action-layout"
        :grid-props="{ gutter: [20, 20], sm: 24, md: 12, lg: 8, xl: 6 }"
        :bordered="false"
        :data="dataList"
        :pagination-props="{
          pageSize: searchConditions.pageSize,
          current: searchConditions.current,
          total,
        }"
        @page-change="onPageChange"
      >
        <template #item="{ item }">
          <AppCard :app="item" :statusColor="reviewStatusMap[item.reviewStatus].color" />
        </template>
      </a-list>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watchEffect } from "vue";
import AppCard from "@/components/AppCard.vue";
import API from "@/api";
import { listAppVoByPageUsingPost } from "@/api/appController";
import message from "@arco-design/web-vue/es/message";
import { REVIEW_STATUS_ENUM } from "@/constant/app";

// 初始化搜索条件（不应该被修改）
const initSearchConditions = {
  current: 1,
  pageSize: 12,
  sortOrder: "descend",
  sortField: "createTime",
};

const searchConditions = ref<API.AppQueryRequest>({
...initSearchConditions,
});
const dataList = ref<API.AppVO[]>([]);
const total = ref<number>(0);

// 定义审核状态枚举值到友好文字及对应颜色的映射函数
const reviewStatusMap = {
  [REVIEW_STATUS_ENUM.PASS]: { text: "已通过审核", color: "#28a745" },
  [REVIEW_STATUS_ENUM.REJECT]: { text: "未通过审核", color: "#dc3545" },
  // 根据实际枚举值补充其他情况
};

// 获取应用数据
const fetchAppData = async () => {
  const params = {
    reviewStatus: REVIEW_STATUS_ENUM.PASS,
  ...searchConditions.value,
  };
  const res = await listAppVoByPageUsingPost(params);
  if (res.data.code === 0) {
    dataList.value = res.data.data?.records || [];
    total.value = res.data.data?.total || 0;
  } else {
    message.error("获取数据失败，" + res.data.message);
  }
};

// 当分页变化时，改变搜索条件，触发数据加载
const onPageChange = (page: number) => {
  searchConditions.value = {
  ...searchConditions.value,
    current: page,
  };
};

// 监听searchConditions变量，改变时触发数据的重新加载
watchEffect(() => {
  fetchAppData();
});
</script>

<style scoped>
#homePage {
  font-family: Arial, sans-serif;
  background-color: #FFF8E1; /* 米白色作为页面整体背景 */
  padding: 80px;
}

.page-header {
  background-color: #FFF8E1; /* 与页面背景一致的米白色 */
  text-align: center;
  margin-bottom: 60px;
}

.page-header h1 {
  font-size: 32px;
  color: #795548; /* 标题使用深棕色，作为强调色突出显示 */
  margin-bottom: 10px;
}

.page-header p {
  font-size: 18px;
  color: #757575; /* 描述文字使用稍深一点的灰色，适配整体风格 */
}

.search-bar-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 50px;
  background-color: #FFF8E1; /* 保持米白色背景 */
}

.search-bar {
  border: 1px solid #BDBDBD; /* 搜索栏边框使用银灰色，作为辅助色，更显柔和 */
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 10px;
  display: flex;
  align-items: center;
  background-color: #FFE0B2; /* 搜索栏主体使用淡橙色，与背景形成对比 */
}

a-input-search::placeholder {
  color: #757575; /* 占位文字使用稍深一点的灰色 */
}

.app-list-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1.2fr));
  gap: 60px;
  background-color: #FFF8E1; /* 保持米白色背景 */
}

.list-demo-action-layout {
  background-color: #FFE0B2; /* 应用卡片背景使用淡橙色，突出内容区域且与背景形成对比 */
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.list-demo-action-layout:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.app-icon {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  background-color: #795548; /* 图标背景使用深棕色作为强调色 */
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 40px;
  color: white;
  font-size: 40px;
  font-weight: bold;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.app-title {
  text-align: center;
  margin-bottom: 20px;
  font-size: 22px;
  font-weight: bold;
  color: #795548; /* 应用标题文字使用深棕色作为强调色 */
  line-height: 1.5;
}

.app-review-status {
  text-align: center;
  font-size: 16px;
  color: #757575; /* 审核状态文字使用稍深一点的灰色 */
}
</style>