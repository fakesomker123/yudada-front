<template>
  <div id="userLayout">
    <a-layout style="height: 100vh">
      <a-layout-header class="header">
        <a-space>
          <img class="logo" src="../assets/logo.png" />
          <div>鱼答答 AI 答题应用平台</div>
        </a-space>
      </a-layout-header>
      <a-layout-content class="content">
        <router-view />
      </a-layout-content>
      <a-layout-footer class="footer">
           by 守得云开见月明
      </a-layout-footer>
    </a-layout>
  </div>
</template>

<script setup lang="ts"></script>

<style scoped>
#userLayout {
  font-family: Arial, sans-serif; /* 设置全局字体 */
  text-align: center;
  background: url("https://gw.alipayobjects.com/zos/rmsportal/FfdJeJRQWjEeGTpqgBKj.png")
    0% 0% / 100% 100%;
  display: flex;
  flex-direction: column; /* 让内部元素按列布局，方便整体样式控制 */
  min-height: 100vh; /* 确保页面至少占满视口高度 */
}

#userLayout.logo {
  width: 80px; /* 进一步增大 logo 尺寸，使其更醒目 */
  height: 80px;
  border-radius: 50%; /* 将 logo 设置为圆形，更美观独特 */
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* 为 logo 添加柔和阴影效果 */
  transition: transform 0.2s ease; /* 添加动画过渡效果，用于鼠标悬停时缩放 */
}

#userLayout.logo:hover {
  transform: scale(1.1); /* 鼠标悬停时 logo 放大一点，增强交互性 */
}

#userLayout.header {
  margin-top: 20px; /* 增大顶部间距 */
  font-size: 28px; /* 再次增大字体大小，使其更突出 */
  background-color: #FFF8E1; /* 设置为米白色，符合暖色调主题 */
  display: flex;
  justify-content: space-between; /* 方便内部元素布局 */
  align-items: center; /* 垂直居中对齐元素 */
  padding: 15px 30px; /* 增加内边距，让元素不显得局促 */
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1); /* 增加柔和的阴影效果 */
  position: relative; /* 为后续添加伪元素装饰做准备 */
}

#userLayout.header::before {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: #FFE0B2; /* 添加一条淡橙色的下划线作为装饰，与整体风格呼应 */
}

#userLayout.header a-space {
  display: flex;
  align-items: center; /* 确保 a-space 内元素垂直居中对齐 */
}

#userLayout.header div {
  color: #795548; /* 标题文字使用深棕色，作为强调色突出显示 */
  font-weight: bold; /* 加粗标题文字，更显重要 */
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1); /* 添加文字阴影，增加立体感 */
}

#userLayout.content {
  margin-bottom: 30px; /* 增大底部间距 */
  padding: 30px; /* 增大内边距，让内容有更多空间展示 */
  background: linear-gradient(to right, #FFF8E1, #FFE0B2); /* 使用暖色调渐变背景，从米白色到淡橙色 */
  flex: 1; /* 让内容区域自动填充剩余空间 */
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* 增加盒子阴影，增加层次感 */
  overflow-y: auto; /* 当内容超出时出现滚动条 */
}

#userLayout.content > div {
  background-color: #FFE0B2; /* 内部内容区域背景设为淡橙色，可根据实际需求微调 */
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); /* 增加盒子阴影，增加层次感 */
  padding: 20px;
  margin-bottom: 20px; /* 每个内容模块之间增加间距 */
  transition: box-shadow 0.2s ease; /* 为盒子阴影变化添加过渡效果 */
}

#userLayout.content > div:hover {
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15); /* 鼠标悬停时阴影加深，增强交互效果 */
}

.footer {
  padding: 20px; /* 增大内边距 */
  text-align: center;
  background-color: #FFF8E1; /* 改为米白色，与整体风格统一 */
  color: #757575; /* 文字颜色设为稍深一点的灰色，适配整体风格 */
  font-size: 14px; /* 适当减小字体大小，显得更精致 */
}

.footer a {
  color: #795548; /* 链接颜色设为深棕色，作为强调色突出显示 */
  text-decoration: none; /* 去掉默认下划线 */
  transition: color 0.2s ease, text-shadow 0.2s ease; /* 添加颜色和文字阴影变化过渡效果 */
  padding: 2px 5px; /* 为链接添加一点内边距，视觉上更舒适 */
  border-radius: 4px; /* 为链接设置圆角，更美观 */
}

.footer a:hover {
  color: #5D4037; /* 链接悬停时颜色加深，增强交互效果 */
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.1); /* 悬停时添加文字阴影，增强立体感 */
}
</style>