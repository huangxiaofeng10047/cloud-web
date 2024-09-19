// 用 vue 实现一个 ai 聊天的对话框
<template>
  <div class="chat-box">
    <!-- 消息列表 -->
    <ul class="message-list">
      <li v-for="(message, index) in messages" :key="index" :class="{ 'me': message.isMe }">
        {{ message.text }}
      </li>
    </ul>

    <!-- 输入框和发送按钮 -->
    <div class="input-group">
      <input type="text" v-model="newMessage" placeholder="Type your message here...">
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      messages: [
        { text: "Hello!", isMe: false },
        { text: "Hi there!", isMe: true }
      ],
      newMessage: ''
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage.trim() !== '') {
        this.messages.push({ text: this.newMessage, isMe: true });
        this.newMessage = ''; // 清空输入框
      }
    }
  }
};
</script>

<style scoped>
.chat-box {
  width: 400px;
  margin: auto;
}

.message-list {
  list-style-type: none;
  padding: 0;
}

.message-list li.me {
  background-color: #DCF8C6;
  align-self: flex-end;
}

.input-group {
  display: flex;
  margin-top: 10px;
}
</style>
