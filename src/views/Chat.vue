<template>
  <div class="chat-page">
    <!-- 引入历史侧边栏 -->
    <HistorySidebar @onSwitchChat="loadHistoryChat" ref="historySidebar" />
    <!-- 聊天主区域 -->
    <div class="chat-main">
      <div class="chat-box">
        <div v-for="(msg, idx) in messages" :key="idx" class="msg-item">
          {{ msg.content }}
        </div>
      </div>
      <div class="input-area">
        <input v-model="inputMsg" @keyup.enter="sendMsg" placeholder="输入消息..." />
        <button @click="sendMsg">发送</button>
      </div>
    </div>
  </div>
</template>

<script>
import HistorySidebar from '../components/historysidebar.vue'
export default {
  components: { HistorySidebar },
  data() {
    return {
      messages: [],
      inputMsg: ''
    }
  },
  methods: {
    sendMsg() {
      if (!this.inputMsg.trim()) return
      // 把消息加入列表
      this.messages.push({ content: this.inputMsg, role: 'user' })
      // 模拟AI回复，实际换成你平台的模型调用
      setTimeout(() => {
        this.messages.push({ content: '我收到你的消息了', role: 'assistant' })
        // 发送完后存进历史对话
        this.saveToHistory()
      }, 500)
      this.inputMsg = ''
    },
    loadHistoryChat(msgs) {
      this.messages = msgs
    },
    saveToHistory() {
      // 用第一条消息当历史对话标题
      const title = this.messages[0]?.content.slice(0, 15) || '新对话'
      this.$refs.historySidebar.addToHistory(title, this.messages)
    }
  }
}
</script>

<style scoped>
.chat-page { display: flex; }
.chat-main { margin-left: 240px; flex: 1; height: 100vh; display: flex; flex-direction: column; }
.chat-box { flex: 1; padding: 20px; overflow-y: auto; background: #f8fafc; }
.input-area { display: flex; padding: 16px; border-top: 1px solid #e2e8f0; }
.input-area input { flex: 1; padding: 10px; border: 1px solid #ddd; border-radius: 8px; margin-right: 10px; }
.input-area button { padding: 10px 20px; background: #409eff; color: white; border: none; border-radius: 8px; cursor: pointer; }
</style>