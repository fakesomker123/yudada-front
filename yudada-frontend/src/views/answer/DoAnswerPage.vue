<template>
  <div id="doAnswerPage">
    <a-card v-if="!loading">
      <div class="card-content">
        <div class="question-container">
          <h1>{{ app.appName }}</h1>
          <p>{{ app.appDesc }}</p>
          <p class="progress-text">答题进度：{{ current }} / {{ questionContent.length }}</p>
          <h2 style="margin-bottom: 16px">
            {{ current }}、{{ currentQuestion?.title }}
          </h2>
        </div>
        <div>
          <a-radio-group
            direction="vertical"
            v-model="currentAnswer"
            :options="questionOptions"
            @change="doRadioChange"
          />
        </div>
        <div style="margin-top: 24px">
          <a-space size="large">
            <a-button
              type="primary"
              circle
              v-if="current < questionContent.length"
              :disabled="!currentAnswer"
              @click="current += 1"
              class="answer-button"
            >
              下一题
            </a-button>
            <a-button
              type="primary"
              v-if="current === questionContent.length"
              :loading="submitting"
              circle
              :disabled="!currentAnswer"
              @click="doSubmit"
              class="answer-button"
            >
              {{ submitting? "评分中" : "查看结果" }}
            </a-button>
            <a-button
              v-if="current > 1"
              circle
              @click="current -= 1"
              class="answer-button"
            >
              上一题
            </a-button>
          </a-space>
        </div>
      </div>
    </a-card>
    <div v-else class="loading-container">
      <p>正在加载题目和应用信息，请稍等...</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import {
  computed,
  defineProps,
  reactive,
  ref,
  watchEffect,
  withDefaults,
} from "vue";
import API from "@/api";
import { useRouter } from "vue-router";
import { listQuestionVoByPageUsingPost } from "@/api/questionController";
import message from "@arco-design/web-vue/es/message";
import { getAppVoByIdUsingGet } from "@/api/appController";
import {
  addUserAnswerUsingPost,
  generateUserAnswerIdUsingGet,
} from "@/api/userAnswerController";

interface Props {
  appId: string;
}

const props = withDefaults(defineProps<Props>(), {
  appId: () => {
    return "";
  },
});

const router = useRouter();

const app = ref<API.AppVO>({});
const questionContent = ref<API.QuestionContentDTO[]>([]);
const current = ref(1);
const currentQuestion = ref<API.QuestionContentDTO>({});
const questionOptions = computed(() => {
  return currentQuestion.value?.options
   ? currentQuestion.value.options.map((option) => {
        return {
          label: `${option.key}. ${option.value}`,
          value: option.key,
        };
      })
    : [];
});
const currentAnswer = ref<string>();
const answerList = reactive<string[]>([]);
const submitting = ref(false);
const id = ref<number>();

const generateId = async () => {
  const res = await generateUserAnswerIdUsingGet();
  if (res.data.code === 0) {
    id.value = res.data.data as any;
  } else {
    message.error("获取唯一 id 失败，" + res.data.message);
  }
};

watchEffect(() => {
  generateId();
});

const loadAppData = async () => {
  if (!props.appId) {
    return;
  }
  const res: any = await getAppVoByIdUsingGet({
    id: props.appId as any,
  });
  if (res.data.code === 0) {
    app.value = res.data.data as any;
  } else {
    message.error("获取应用失败，" + res.data.message);
  }
};

const loadQuestionData = async () => {
  if (!props.appId) {
    return;
  }
  const res = await listQuestionVoByPageUsingPost({
    appId: props.appId as any,
    current: 1,
    pageSize: 1,
    sortField: "createTime",
    sortOrder: "descend",
  });
  if (res.data.code === 0 && res.data.data?.records) {
    questionContent.value = res.data.data.records[0].questionContent;
  } else {
    message.error("获取题目失败，" + res.data.message);
  }
};

watchEffect(() => {
  loadAppData();
  loadQuestionData();
});

watchEffect(() => {
  currentQuestion.value = questionContent.value[current.value - 1];
  currentAnswer.value = answerList[current.value - 1];
});

const doRadioChange = (value: string) => {
  answerList[current.value - 1] = value;
};

const doSubmit = async () => {
  if (!props.appId ||!answerList) {
    return;
  }
  submitting.value = true;
  const res = await addUserAnswerUsingPost({
    appId: props.appId as any,
    choices: answerList,
    id: id.value as any,
  });
  if (res.data.code === 0 && res.data.data) {
    router.push(`/answer/result/${res.data.data}`);
  } else {
    message.error("提交答案失败，" + res.data.message);
  }
  submitting.value = false;
};
</script>

<style scoped>
#doAnswerPage {
  padding: 20px;
}

.card-content {
  padding: 20px;
}

.question-container {
  padding: 20px;
  background-color: #F5F5F5;
  border-radius: 8px;
  margin-bottom: 20px;
}

.progress-text {
  font-size: 16px;
  color: #757575;
  margin-bottom: 10px;
}

h2 {
  font-weight: bold;
  font-size: 20px;
}

.a-radio-group {
  padding: 15px;
  border: 1px solid #BDBDBD;
  border-radius: 8px;
}

.answer-button {
  width: 40px;
  height: 40px;
  font-size: 18px;
  background-color: #795548;
  border-color: #795548;
  color: white;
  transition: background-color 0.2s ease, border-color 0.2s ease, box-shadow 0.2s ease;
}

.answer-button:hover {
  background-color: #5D4037;
  border-color: #5D4037;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.loading-container {
  text-align: center;
  margin-top: 50px;
  font-size: 18px;
}
</style>