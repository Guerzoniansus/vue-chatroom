<template>
  <div class="root">
    <div v-if="! username">
      <UsernameInput @submittedUsername="connect"/>
    </div>

    <div v-else>
      <p>Username: {{ username }}</p>
      <div class="message-container">
        <p class="message" v-for="(message, i) in messages" :key="i">
          <span class="username">{{ message.username }}:</span> {{ message.message }}
        </p>
      </div>
      <br/>
      <input v-model="input" @keyup.enter="sendMessage" placeholder="Enter message...">
    </div>
  </div>
</template>

<script setup>
import {ref} from "vue";
import UsernameInput from "@/components/UsernameInput";

const username = ref("");
const input = ref("");
const messages = ref([]);

let socket;

function connect(submittedUsername) {
  username.value = submittedUsername;

  socket = new WebSocket("ws://localhost:8080/chat/" + username.value);
  socket.onmessage = (event) => messages.value.push(JSON.parse(event.data));
}

function sendMessage() {
  socket.send(input.value);
  input.value = "";
  console.log(messages.value);
}
</script>

<style scoped>
.root {
  width: 500px;
  margin: auto;
}

.message-container {
  border: 1px solid grey;
}

.username {
  font-weight: bold;
}

.message{
  text-align: left;
  margin-left: 10px;
}
</style>