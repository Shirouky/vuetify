<template>
  <div id="app">
    <Container>
      <ChatWindow :text="text" :author="author" @send-data="sendMessage">
        <div v-for="item in messages" :key="item.id">
         <ChatMessage :author="item.author" :datetime="item.datetime" :text="item.text"></ChatMessage>
        </div>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
import Container from './components/Container.vue'
import ChatMessage from './components/ChatMessage.vue'
import ChatWindow from './components/ChatWindow.vue'

export default {
  name: 'App',
  components: {
    Container,
    ChatMessage,
    ChatWindow
  },
  mounted(){
    setInterval(() => {
      this.loadMessage();
    }, 1000 * 60)
  },
  methods: {
    loadMessage() {
      this.axios.get('https://61bcd385d8542f0017824a2a.mockapi.io/messages')
      .then((response) => {
          this.messages = response.data;
      })
    },
    sendMessage(data){
      console.log("send-data", data)
      this.axios.post('https://61bcd385d8542f0017824a2a.mockapi.io/messages', data)
      .then((response) => {
          this.messages = response.data;
      })
    }
  },
  data() {
    return {
      author: "",
      text: "",
      messages: [],
    };
  },
}
</script>

<style>
body {
  margin: 0;
  background-color: #f9f9fa;
}
</style>
