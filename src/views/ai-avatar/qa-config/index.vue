<script>
export default {
  data() {
    return {
      // 聊天模块数据
      chatMessages: [
        {
          type: 'user',
          content: '你好'
        },
        {
          type: 'system',
          content: '您好，欢迎使用物业服务智能问答系统！我是您的数字物业助理，请问需要什么帮助？'
        },
        {
          type: 'user',
          content: '有哪些社区活动可以参加？'
        },
        {
          type: 'system',
          content:
            '本月社区活动有以下安排：\n1. 健身操培训（每周二、四，下午6:00-7:00）；\n2. 社区义卖活动（3月18日，上午9:00-12:00）；\n3. 业主联谊晚会（3月25日，晚上7:00-9:00）。\n欢迎参加！'
        }
      ],

      userInput: '',

      // 配置模块数据
      selectedPrompt: 'general',
      prompts: [
        { value: 'general', label: '通用对话' },
        { value: 'repair', label: '维修请求生成' },
        { value: 'activity', label: '活动安排生成' }
      ],
      contextEnabled: false,
      contextLength: 3,
      temperature: 0.7,
      topK: 50,
      topP: 0.9,
      knowledgeSource: 'local',
      syncKnowledgeBase: true,
      logEnabled: true,
      feedbackEnabled: true,
      defaultReply: '抱歉，我无法回答您的问题。'
    };
  },
  methods: {
    sendMessage() {
      if (!this.userInput.trim()) return;
      const userMessage = { type: 'user', content: this.userInput };
      this.chatMessages.push(userMessage);
      this.userInput = '';

      setTimeout(() => {
        const response = this.generateResponse(userMessage.content);
        this.chatMessages.push({ type: 'system', content: response });
      }, 500);
    },
    generateResponse(input) {
      // 根据选定的 Prompt 生成回复
      switch (this.selectedPrompt) {
        case 'repair':
          return `维修请求已生成：${input}`;
        case 'activity':
          return `活动安排已生成：${input}`;
        default:
          return `已收到您的问题：${input}`;
      }
    },
    editPrompt() {
      alert('编辑 Prompt 模板功能尚未实现');
    }
  }
};
</script>

<template>
  <div class="app-container">
    <!-- 左侧聊天模块 -->
    <div class="chat-container">
      <div class="chat-header">问答聊天模块</div>
      <div class="chat-body">
        <div v-for="(message, index) in chatMessages" :key="index" class="chat-message" :class="message.type">
          <div class="message-bubble">
            <p>{{ message.content }}</p>
          </div>
        </div>
      </div>
      <div class="chat-footer">
        <input v-model="userInput" type="text" placeholder="请输入内容..." @keyup.enter="sendMessage" />
        <button @click="sendMessage">发送</button>
      </div>
    </div>

    <!-- 右侧配置模块 -->
    <div class="config-container">
      <div class="config-header">问答配置模块</div>
      <div class="config-body">
        <!-- Prompt 模板选择 -->
        <div class="config-item">
          <label for="prompt">选择 Prompt 模板：</label>
          <select id="prompt" v-model="selectedPrompt">
            <option v-for="prompt in prompts" :key="prompt.value" :value="prompt.value">
              {{ prompt.label }}
            </option>
          </select>
          <AButton style="margin-top: 5px" size="small">编辑模板</AButton>
        </div>

        <!-- 问答上下文管理 -->
        <div class="config-item">
          <label>上下文长度：</label>
          <input v-model="contextLength" type="number" min="1" max="10" placeholder="输入上下文轮次" />
        </div>

        <!-- 知识库管理 -->
        <div class="config-item">
          <label>知识库来源：</label>
          <select v-model="knowledgeSource">
            <option value="local">本地知识图谱</option>
            <option value="cloud">云端知识库</option>
            <option value="static">静态预设数据</option>
          </select>
        </div>

        <!-- 错误处理策略 -->
        <div class="config-item">
          <label>无法回答时的默认回复：</label>
          <input v-model="defaultReply" type="text" placeholder="如：抱歉，我无法回答您的问题" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* 应用主容器 */
.app-container {
  display: flex;
  height: 100vh;
  background-color: #f0f0f0; /* 灰色背景 */
  font-family: Arial, sans-serif;
}

/* 聊天模块样式 */
.chat-container {
  flex: 2;
  display: flex;
  flex-direction: column;
  border-right: 1px solid #ccc;
  background-color: #ffffff;
}

.chat-header {
  padding: 15px;
  background-color: #3098ff;
  color: #fff;
  font-weight: bold;
  font-size: 18px;
}

.chat-body {
  flex: 1;
  padding: 15px;
  overflow-y: auto;
  background-color: #f9f9f9;
}

.chat-message {
  margin-bottom: 10px;
}

.chat-message.user {
  text-align: right;
  color: #3098ff;
}

.chat-message.system {
  text-align: left;
  color: #444;
}

.message-bubble {
  display: inline-block;
  padding: 10px;
  border-radius: 8px;
  background-color: #e6f7ff;
  max-width: 60%;
}

.chat-footer {
  display: flex;
  padding: 10px;
  background-color: #f5f5f5;
  border-top: 1px solid #ccc;
}

.chat-footer input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin-right: 10px;
  background-color: #fff;
}

.chat-footer button {
  background-color: #3098ff;
  color: #fff;
  border: none;
  padding: 10px 15px;
  border-radius: 5px;
  cursor: pointer;
}

.chat-footer button:hover {
  background-color: #005a9e;
}

/* 配置模块样式 */
.config-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  background-color: #f9f9f9;
}

.config-header {
  padding: 15px;
  background-color: #3098ff;
  color: #fff;
  font-weight: bold;
  font-size: 18px;
}

.config-body {
  flex: 1;
  padding: 15px;
  overflow-y: auto;
}

.config-item {
  margin-bottom: 15px;
}

.config-item label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
}

.config-item input,
.config-item select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #fff;
}
</style>
