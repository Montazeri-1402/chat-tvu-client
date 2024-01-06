<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue';
import axios from 'axios';
import { ref } from 'vue';
import router from './router';

const currentUserId = 1;

let users = ref([]);
axios.get('/api/users')
  .then((res) => {
    console.log({ res });
    users.value = res.data;
  });

let conversations = ref([]);
axios.get('/api/conversations/latest', {
  params: {
    currentUserId,
  }
}).then((res) => {
  console.log({ res });
  conversations.value = res.data;
});

const openConversations = (conversation) => {
  console.log({ conversation });
  router.push({ name: "conversation", params: { conversationId: conversation.id } })
}
</script>

<template>
  <div v-for="user of users" :key="user.id">
    {{ user.fullname }}
  </div>

  <div class="conversations">
    <div class="conversation" v-for="conversation of conversations" :key="conversation.id"
      @click="() => openConversations(conversation)">
      <div class="target">
        {{ conversation.senderUserId == currentUserId ? conversation.receiverUserId : conversation.senderUserId }}
      </div>
      <div class="latest">
        {{ conversation._latestChat?.message }}
        {{ conversation._latestChat?.createdAt }}
      </div>
    </div>
  </div>

  <RouterView />
</template>

<style scoped>
.conversation {
  border: solid thin black;
  padding: 10px;
  cursor: pointer;
  margin-bottom: 10px;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
