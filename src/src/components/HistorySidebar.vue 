<template>
  <div class="history-sidebar">
    <div class="sidebar-top">
      <h3>历史对话</h3>
      <button @click="startNewChat" class="new-btn">+ 新建对话</button>
    </div>
    <div class="chat-list">
      <div 
        v-for="(item, idx) in historyList" 
        :key="idx"
        class="chat-item"
        :class="{active: currentIdx === idx}"
        @click="loadHistory(idx)"
      >
        <span class="chat-title">{{ item.title }}</span>
        <button class="del-btn" @click.stop="deleteHistory(idx)">×</button>
      </div>
      <div v-if="historyList.length === 0" class="empty-tip">暂无历史对话</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HistorySidebar',
  data() {
    return {
      historyList: [], // 存历史对话列表
      currentIdx: -1   // 当前选中的对话索引
    }
  },
  mounted() {
    // 页面加载时从本地缓存读取历史
    const saved = localStorage.getItem('myChatHistory')
    if (saved) this.historyList = JSON.parse(saved)
  },
  methods: {
    // 保存历史到本地缓存
    saveHistory() {
      localStorage.setItem('myChatHistory', JSON.stringify(this.historyList))
    },
    // 新建对话，通知父页面清空内容
    startNewChat() {
      this.currentIdx = -1
      this.$emit('onSwitchChat', []) // 向对话页发送空内容
    },
    // 加载选中的历史对话
    loadHistory(idx) {
      this.currentIdx = idx
      this.$emit('onSwitchChat', this.historyList[idx].messages)
    },
    // 删除单条历史
    deleteHistory(idx) {
      this.historyList.splice(idx, 1)
      this.saveHistory()
    },
    // 供父页面调用：把新对话存进历史
    addToHistory(chatTitle, messages) {
      // 如果是当前对话更新，就覆盖，否则新增
      if (this.currentIdx > -1) {
        this.historyList[this.currentIdx].messages = messages
        this.historyList[this.currentIdx].title = chatTitle
      } else {
        this.historyList.unshift({ title: chatTitle, messages })
        this.currentIdx = 0
      }
      this.saveHistory()
    }
  }
}
</script>

<style scoped>
.history-sidebar {
  width: 240px;
  height: 100vh;
  background: #1e293b;
  border-right: 1px solid #334155;
  padding: 16px;
  box-sizing: border-box;
  position: fixed;
  left: 0;
  top: 0;
}
.sidebar-top {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
.sidebar-top h3 {
  color: #fff;
  margin: 0;
  font-size: 16px;
}
.new-btn {
  background: #409eff;
  border: none;
  color: white;
  padding: 6px 12px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
}
.chat-list {
  overflow-y: auto;
  height: calc(100vh - 80px);
}
.chat-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 12px;
  margin: 8px 0;
  border-radius: 8px;
  background: rgba(255,255,255,0.05);
  cursor: pointer;  transition: background 0.2s;
}
.chat-item:hover {
  background: rgba(255,255,255,0.1);
}
.chat-item.active {
  background: #409eff;
}
.chat-title {
  color: #e2e8f0;
  font-size: 14px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  flex: 1;
}
.chat-item.active .chat-title {
  color: #fff;
}
.del-btn {
  background: transparent;
  border: none;
  color: #94a3b8;
  font-size: 16px;
  cursor: pointer;
  padding: 0 4px;
  margin-left: 8px;
}
.del-btn:hover {
  color: #ff4757;
}
.empty-tip {
  color: #64748b;
  font-size: 13px;
  text-align: center;
  padding-top: 40px;
}
</style>