<template>
  <div class="chat-container">
    <div class="user-selector">
      <select v-model="selectUser">
        <option v-for="user in users" :key="user.name">{{user.name}}</option>
      </select>
    </div>
    <div ref="messagesBox" class="messages-box">
      <div v-for="message in messages" :key="message.messageID" 
         :class="['message', (message.isMe && selectUser === 'Me') || (!message.isMe && selectUser === 'Other') ? 'Me' : 'Other']">
        <img :src="message.avatar" alt="avatar" class="avatar"/>
        <div class="message-content">{{ message.text }}</div>
      </div>
    </div>
    <div class="input-area">
      <input type="text" v-model="inputText" @keydown.ctrl.enter="sendMessage"  maxlength="1000" placeholder="请输入消息" />
      <button @click="sendMessage" :disabled="ISsendButonDisabled">发送</button>
    </div>
  </div>
</template>

<script>
export default {
  
  data() {
    return {
      messages : [],
      inputText : '',
      userAvatar : '',
      selectUser : '',
      messageId : 0,
      ISsendButonDisabled : true,
      users:[
        {id : 1,name: 'Me', avatar: require('../assets/dog.jpg')},
        {id : 2,name: 'Other', avatar: require('../assets/cat.jpg')}
      ]
    };
  },

  watch : {
    inputText(val){
      this.ISsendButonDisabled = val.trim() === ''
    }
  },

  methods: {
    sendMessage() {
      if (this.inputText.trim() === '') return;

      let currentUser = this.users.find(user => user.name === this.selectUser)

      this.messages.push({
        messageID : this.messageId++,
        id: currentUser.id,
        sender: currentUser.name,
        text: this.inputText,
        avatar: currentUser.avatar,
        isMe : this.selectUser === 'Me'
      });

      this.inputText = '';
      this.$nextTick(() => {
        this.$refs.messagesBox.scrollTop = this.$refs.messagesBox.scrollHeight;
      });
    }
  }
}
</script>

<style>
.chat-container {
  background-color: white;
  padding: 12px;
  max-width: 600px;
  box-shadow: 2px 2px 6px rgb(0, 0, 0, 0.1);
}

.messages-box {
  height: 400px;
  padding: 8px;
  overflow-y: auto;
}

.message {
  display: flex;
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  margin-right: 8px;
}

.message-content {
  background-color: #e2d2d2;
  padding: 10px;
  border-radius: 8px;
  max-width: 33%;
  margin-bottom: 10px;
  word-break: break-word;
}

.message.me .message-content {
  background-color: #3498db;
  color: white;
}

.input-area {
  display: flex;
  align-items: center;
}

input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  height: 48px;
}

button {
  margin-left: 10px;
  padding: 8px 16px;  /* 增加水平内边距，使得按钮看起来更宽敞 */
  border: none;
  height: 48px;
  background-color: #3498db;
  color: white;
  cursor: pointer;
  border-radius: 4px; /* 输入框有圆角，所以给按钮也加上相同的圆角 */
  box-sizing: border-box;
  font-size: 16px; /* 调整字体大小以匹配输入框的字体大小 */
}

.message.Me{
  justify-content: flex-end;
}

.message.Me .avatar{
  order: 1;
  margin-left: 8px;
  margin-right: 0;
}

.message.Other{
  justify-content: flex-start;
}

.message.Other .avatar{
  order: 0;
  margin-right: 8px;
  margin-left: 0;
}

button:disabled{
  background: #b6abab;
  cursor: not-allowed;
  color: #776d6d;
}

.user-selector{
  margin-top: 12px;
}
</style>
