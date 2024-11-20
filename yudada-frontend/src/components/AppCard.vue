<template>
  <a-card class="appCard" hoverable @click="doCardClick">
    <template #actions>
      <span class="icon-hover" @click="doShare" :style="{ position: 'absolute', bottom: '10px', right: '10px' }"> <IconShareInternal /> </span>
    </template>
    <template #cover>
      <div class="app-icon-placeholder">
        <div class="app-icon">
          {{ app.appName.charAt(0).toUpperCase() }}
        </div>
      </div>
    </template>
    <a-card-meta :title="app.appName" :description="app.appDesc">
      <template #avatar>
        <!-- 头像和用户名垂直排列，调整样式属性 -->
        <div
          :style="{ display: 'flex', flexDirection: 'column', alignItems: 'center', color: '#1D2129' }"
        >
          <a-avatar
            :size="32"
            :image-url="app.user?.userAvatar"
            :style="{ marginBottom: '5px' }"
          />
          <a-typography-text
            :style="{ fontSize: '18px', lineHeight: '1.5' }"
            >{{ app.user?.userName?? "无名" }}
          </a-typography-text>
        </div>
      </template>
    </a-card-meta>
  </a-card>
  <ShareModal :link="shareLink" title="应用分享" ref="shareModalRef" />
</template>

<script setup lang="ts">
import { IconShareInternal } from "@arco-design/web-vue/es/icon";
import API from "@/api";
import { defineProps, ref, withDefaults } from "vue";
import { useRouter } from "vue-router";
import ShareModal from "@/components/ShareModal.vue";

interface Props {
  app: API.AppVO;
}

const props = withDefaults(defineProps<Props>(), {
  app: () => {
    return {};
  },
});

const router = useRouter();
const doCardClick = () => {
  router.push(`/app/detail/${props.app.id}`);
};

// 分享弹窗的引用
const shareModalRef = ref();

// 分享链接
const shareLink = `${window.location.protocol}//${window.location.host}/app/detail/${props.app.id}`;

// 分享
const doShare = (e: Event) => {
  if (shareModalRef.value) {
    shareModalRef.value.openModal();
  }
  // 阻止冒泡，防止跳转到详情页
  e.stopPropagation();
};
</script>

<style scoped>
.appCard {
  cursor: pointer;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  background-color: #FFE0B2; /* 应用卡片整体背景使用淡橙色，与主页风格统一 */
  background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.1) 25%, transparent 25%, transparent 50%, rgba(255, 255, 255, 0.1) 50%, rgba(255, 255, 255, 0.1) 75%, transparent 75%, transparent);
  background-size: 16px 16px;
  background-repeat: repeat;
  margin-bottom: 30px;
  width: 350px;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
  position: relative;
}

.appCard::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: #BDBDBD; /* 卡片顶部装饰线使用银灰色，作为辅助色 */
}

.appCard:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16pxrgba(0, 0, 0, 0.2);
}

.app-icon-placeholder {
  width: 100%;
  height: 220px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
}

.app-icon {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background-color: #795548; /* 图标背景保持深棕色作为强调色 */
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 36px;
  font-weight: bold;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  position: relative;
  z-index: 1;
  filter: drop-shadow(0 0 5px rgba(255, 255, 255, 0.5)) hue-rotate(20deg);
}

.app-icon-placeholder::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: radial-gradient(
    ellipse at center,
    rgba(231, 76, 60, 0.2) 0%,
    rgba(231, 76, 60, 0) 70%
  );
  z-index: 0;
}

.app-icon-placeholder::after {
  content: "";
  position: absolute;
  bottom: 0;
  right: 0;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: radial-gradient(
    ellipse at center,
    rgba(142, 68, 173, 0.2) 0%,
    rgba(142, 68, 173, 0) 70%
  );
  z-index: 0;
}

.icon-hover {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
  border-radius: 50%;
  transition: all 0.1s;
  background-color: rgba(255, 255, 255, 0.8);
}

.icon-hover:hover {
  background-color: rgba(216, 216, 216, 0.6);
}

.appCard.a-card-meta {
  padding: 20px 25px;
  text-align: center;
  width: 100%;
}

.appCard.a-card-meta :title {
  color: #795548; /* 应用卡片内标题文字使用深棕色作为强调色 */
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 5px;
}

.appCard.a-card-meta :description {
  color: #757575; /* 应用卡片内描述文字使用稍深一点的灰色 */
  font-size: 16px;
  line-height: 1.4;
}
</style>