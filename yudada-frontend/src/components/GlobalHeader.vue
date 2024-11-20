<template>
  <a-row id="globalHeader" align="center" :wrap="false">
    <a-col flex="auto">
      <a-menu
        mode="horizontal"
        :selected-keys="selectedKeys"
        @menu-item-click="doMenuClick"
      >
        <a-menu-item
          key="0"
          :style="{ padding: 0, marginRight: '38px' }"
          disabled
        >
          <div class="titleBar">
            <img class="logo" src="../assets/logo.png" />
            <div class="title">灵启答苑</div>
          </div>
        </a-menu-item>
        <a-menu-item v-for="item in visibleRoutes" :key="item.path">
          {{ item.name }}
        </a-menu-item>
      </a-menu>
    </a-col>
    <a-col flex="100px">
      <div v-if="loginUserStore.loginUser.id">
        {{ loginUserStore.loginUser.userName?? "无名" }}
      </div>
      <div v-else>
        <a-button type="primary" href="/user/login">登录</a-button>
      </div>
    </a-col>
  </a-row>
</template>


<script setup lang="ts">
import { routes } from "@/router/routes";
import { useRouter } from "vue-router";
import { computed, ref } from "vue";
import { useLoginUserStore } from "@/store/userStore";
import checkAccess from "@/access/checkAccess";

const loginUserStore = useLoginUserStore();

const router = useRouter();
// 当前选中的菜单项
const selectedKeys = ref(["/"]);
// 路由跳转时，自动更新选中的菜单项
router.afterEach((to, from, failure) => {
  selectedKeys.value = [to.path];
});

// 展示在菜单栏的路由数组
const visibleRoutes = computed(() => {
  return routes.filter((item) => {
    if (item.meta?.hideInMenu) {
      return false;
    }
    // 根据权限过滤菜单
    if (!checkAccess(loginUserStore.loginUser, item.meta?.access as string)) {
      return false;
    }
    return true;
  });
});

// 点击菜单跳转到对应页面
const doMenuClick = (key: string) => {
  router.push({
    path: key,
  });
};
</script>

<style scoped>
#globalHeader {
  background-color: #FFF8E1; /* 设置为米白色，符合暖色调主题 */
  padding: 10px 20px; /* 增加内边距，让元素有合适空间展示 */
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1); /* 添加柔和的阴影效果，增加层次感 */
  position: relative; /* 为了让伪元素能基于该行元素定位 */
}

@media screen and (min-width: 768px) {
    a-row#globalHeader {
        justify-content: space-between; /* 在中等及以上屏幕尺寸，让菜单和用户信息区域拉开距离，分布在两端 */
    }
    a-col.flex-auto {
        position: relative; /* 让菜单项所在列也能相对定位，方便设置伪元素的显示位置 */
    }
    a-col.flex-auto::after {
        content: "";
        position: absolute;
        right: -15px; /* 根据实际间距效果调整此值，控制竖线离菜单列的距离 */
        top: 50%; /* 竖线垂直居中 */
        transform: translateY(-50%);
        width: 1px; /* 竖线宽度 */
        height: 20px; /* 竖线高度，可根据实际情况调整 */
        background-color: #BDBDBD; /* 竖线颜色，选择一个与整体风格适配的较浅灰色 */
    }
}

@media screen and (max-width: 767px) {
    a-row#globalHeader {
        flex-direction: column; /* 在小屏幕时，让菜单和用户信息区域垂直排列 */
        align-items: center; /* 垂直排列时，让元素居中对齐 */
    }
    a-col.flex-auto {
        margin-bottom: 15px; /* 小屏幕下，适当增大菜单下方与用户信息区域的间距，可根据实际效果调整此值 */
    }
    a-col.flex-auto::after {
        display: none; /* 在小屏幕下隐藏竖线，避免影响小屏幕布局效果 */
    }
}

.titleBar {
    display: flex;
    align-items: center;
    gap: 10px; /* 使用 gap 属性，给 logo 和标题之间添加间距，比 margin-left 更灵活方便 */
}

.title {
    color: #795548; /* 标题文字使用深棕色，作为强调色突出显示 */
    font-size: 24px; /* 适当增大字体大小 */
}

.logo {
    height: 48px;
    width: auto; /* 保持 logo 的宽高比自适应，避免变形 */
    border-radius: 50%; /* 保持 logo 的圆形效果 */
    transition: transform 0.2s ease; /* 添加过渡效果，用于鼠标悬停时的缩放动画 */
}

.logo:hover {
    transform: scale(1.1); /* 鼠标悬停时，logo 放大 10%，增加交互效果 */
}

a-menu {
    background-color: rgba(255, 255, 255, 0.8); /* 给菜单添加半透明的白色背景，增加层次感 */
    border-radius: 8px; /* 增加菜单整体的圆角效果，使其看起来更柔和 */
    padding: 5px 10px; /* 增加菜单内边距，让菜单项有更合适的空间展示 */
    display: flex;
    align-items: center;
}

a-menu-item {
    color: #757575;
    font-size: 16px;
    padding: 8px 12px; /* 适当调整菜单项内边距，让文字展示更舒适 */
    border-radius: 4px;
    transition: color 0.2s ease, background-color 0.2s ease;
    margin-right: 15px; /* 增加菜单项之间的水平间距，避免过于拥挤 */
}

a-menu-item:hover {
    color: #5D4037;
    background-color: rgba(255, 234, 178, 0.3); /* 悬停时添加淡淡的背景色变化，增强交互效果 */
}

div[v-if="loginUserStore.loginUser.id"] {
    color: #795548;
    font-size: 16px;
    font-weight: bold; /* 加粗用户信息文字，使其更突出 */
    margin-right: 10px; /* 添加右侧间距，与其他元素隔开一点 */
}

a-button {
    background-color: #795548;
    border-color: #795548;
    color: white;
    font-size: 16px;
    padding: 10px 20px; /* 适当增大按钮的内边距，让按钮看起来更饱满 */
    border-radius: 6px; /* 增加按钮的圆角效果，使其更柔和美观 */
    transition: background-color 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease;
}

a-button:hover {
    background-color: #5D4037;
    border-color: #5D4037;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* 悬停时添加淡淡的阴影效果，增强立体感和点击引导性 */
}
</style>