<template>
  <div id="app">
    <Container>
      <ChatWindow @send-message="sendMessages">
        <ChatMessage
          v-for="message in messages"
          :key="message.id"
          :username="message.author"
          :datetime="message.datetime"
        >{{message.text}}</ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
import Container from "./components/Container.vue";
import ChatWindow from "./components/ChatWindow.vue";
import ChatMessage from "./components/ChatMessage.vue";
import axios from "axios";

export default {
  name: "app",
  components: {
    Container,
    ChatWindow,
    ChatMessage
  },
  data() {
    return {
      messages: []
    }
  },
  methods: {
    sendMessages(obj) {
      axios.post("http://188.225.47.187/api/chat/sendmessage.php", {
          author: obj.nickname,
          text: obj.message
        })
        .then(() => {
          this.getMessages()
        });
    },
    getMessages(){
      axios.get('http://188.225.47.187/api/chat/getmessages.php').then((response)=>{
        this.messages=response.data.sort((a,b) => a.id - b.id)
    })
    },
  },
  mounted() {
    setInterval(() => {
      this.getMessages();
    }, 3000);
  }
};
</script>

<style>
body {
  margin: 0;
  background-color: #f9f9fa;
}
</style>
