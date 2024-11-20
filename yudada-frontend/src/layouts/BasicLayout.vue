<template>
  <div id="basicLayout">
    <a-layout style="height: 100vh">
      <a-layout-header class="header">
        <GlobalHeader />
      </a-layout-header>
      <a-layout-content class="content">
        <router-view />
      </a-layout-content>
      <a-layout-footer class="footer">
         守得云开见月明 hihihi!
      </a-layout-footer>
    </a-layout>
  </div>
</template>

<script setup lang="ts">
import GlobalHeader from "@/components/GlobalHeader.vue";
</script>

<style scoped>
#basicLayout {
  font-family: Arial, sans-serif; /* 设置全局字体 */
}

#basicLayout.header {
  margin-bottom: 20px; /* 增大与内容区域的间距 */
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1); /* 调整阴影效果，更柔和自然 */
  background-color: #FFF8E1; /* 改为米白色，与整体暖色调主题匹配 */
  display: flex;
  justify-content: space-between; /* 可以方便在头部放置不同元素进行左右布局 */
  align-items: center; /* 垂直居中对齐元素 */
  padding: 10px 20px; /* 增加内边距，让元素不显得局促 */
}

#basicLayout.content {
  max-width: 1200px;
  width: 100%;
  box-sizing: border-box;
  margin: 0 auto 30px; /* 增大底部间距 */
  padding: 30px; /* 增大内边距，让内容有更多空间展示 */
  background: linear-gradient(to right, #FFF8E1, #FFE0B2); /* 使用暖色调渐变背景，从米白色到淡橙色 */
}

#basicLayout.content > div {
  background-color: #FFE0B2; /* 内部内容区域背景设为淡橙色，可根据实际需求微调 */
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* 增加盒子阴影，增加层次感 */
  padding: 20px;
  margin-bottom: 20px; /* 每个内容模块之间增加间距 */
}

.footer {
  padding: 20px; /* 增大内边距 */
  text-align: center;
  background: #efefef;
  background-color: #FFF8E1; /* 改为米白色，与整体风格统一 */
  color: #757575; /* 文字颜色设为稍深一点的灰色，适配整体风格 */
}

.footer a {
  color: #795548; /* 链接颜色设为深棕色，作为强调色突出显示 */
  text-decoration: none; /* 去掉默认下划线 */
  transition: color 0.2s ease; /* 添加颜色变化过渡效果 */
}

.footer a:hover {
  color: #5D4037; /* 链接悬停时颜色加深，增强交互效果 */
}
</style>