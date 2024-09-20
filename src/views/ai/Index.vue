<template>
  <div id="index">
    <el-container fluid>
      <el-main>
        <div class="chat-box">
          <div v-for="(message, index) in messages" :key="index" :class="['message', { me: message.isMe }]">
            <span>{{ message.text }}</span>
          </div>
        </div>
        <el-input
          v-model="newMessage"
          placeholder="Type your message"
          @keyup.enter="sendMessage"
        >
          <template #append>
            <el-button @click="sendMessage">Send</el-button>
          </template>
        </el-input>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      messages: [
        { text: 'Hello there!', isMe: false },
        { text: 'Hi! How are you?', isMe: true }
      ],
      newMessage: ''
    };
  },
  methods: {
    async sendMessage() {
      if (this.newMessage.trim()) {
        const message = { text: this.newMessage, isMe: true };

        // 将消息添加到前端显示列表
        this.messages.push(message);

        // 发送请求到后端API
        try {
          const response=await this.sendMsg(message);
          // alert(response);
          if (response) {
            // alert(response);
            this.messages.push({ text: response, isMe: false });
          }
          this.newMessage = ''; // 清空输入框
        } catch (error) {
          console.error('Error sending message:', error);
        }
      }
    },
    async sendMsg(message) {
      const self = this;

     return  await self.$http.post('/chat/chat', message, 'apiUrl', { body: 'json' }).then((res) => {
        if (res === false || res === undefined) {
          return false;
        }
        self.$message({
          message: 'Message sent successfully!',
          type: 'success'
        });
        return res;
      }).catch((error) => {
        console.error('Error sending message:', error);
        throw error;
      });
    }
  }
};
</script>

<style scoped>
.chat-box {
  height: 400px;
  overflow-y: scroll;
  border: 1px solid #ccc;
  padding: 10px;
}

.message {
  padding: 5px 10px;
  border-radius: 5px;
  margin-bottom: 10px;
  display: inline-block;
}

.me {
  background-color: #00bcd4;
  color: white;
  float: right;
}

.not-me {
  background-color: #f5f5f5;
  color: black;
  float: left;
}
</style>