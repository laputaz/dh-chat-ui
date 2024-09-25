<template>
  <header>
    <n-gradient-text type="info" :size="30"> DH&nbsp;</n-gradient-text>
    <n-gradient-text type="danger" :size="30"> CHAT AI </n-gradient-text>
  </header>
  <main>
    <n-input
      v-model:value="content"
      round
      class="dh-input"
      type="textarea"
      size="large"
      placeholder="请输入提问"
    >
      <!-- <template #prefix>
        <n-icon :component="FlashOutline" />
      </template> -->
    </n-input>
    <n-button v-if="!isLoading" type="primary" @click="handleAsk">提问</n-button>
    <n-button v-else type="primary" disabled>请稍候</n-button>
    <n-divider> 提问 </n-divider>
    <div class="answer">
      {{ curAsk }}
    </div>
    <n-divider> 答案 </n-divider>
    <div class="answer">
      {{ answer }}
    </div>
  </main>
</template>
<script lang="ts">
import { ref } from 'vue'
import { NButton, NInput, NDivider, NGradientText } from 'naive-ui'
import { FlashOutline } from '@vicons/ionicons5'
import axios from 'axios'
export default {
  components: { NButton, NInput, NDivider, NGradientText },
  setup() {
    const content = ref('')
    const answer = ref('-')
    const curAsk = ref('您还没有提问')
    const isLoading = ref(false)
    const handleAsk = async () => {
      if (!content.value) {
        return
      }
      isLoading.value = true
      curAsk.value = content.value
      content.value = ''
      answer.value = '思考中'
      try {
        const response = await axios.post('http://192.168.31.103:3000/ask', {
          content: curAsk.value
        })
        answer.value = response?.data.content || '-'
      } finally {
        isLoading.value = false
      }
    }
    return {
      content,
      answer,
      handleAsk,
      FlashOutline,
      curAsk,
      isLoading
    }
  }
}
</script>
<style>
.dh-input {
  margin: 10px 0;
}
.dh-answer {
  padding: 0 10px;
}
</style>
