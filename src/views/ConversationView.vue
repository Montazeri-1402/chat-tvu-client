<template>
    <div class="chats">
        <div class="chat" v-for="chat of chats">
            {{ chat.message }}
            {{ chat.createdAt }}
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { computed, onMounted, ref, watch } from 'vue';
import { useRoute } from "vue-router";

const route = useRoute();
const params = route.params;

const chats = ref([]);

onMounted(async () => {
    watch(() => route.params.conversationId, async (newValue) => {
        const res = await axios.get('/api/chats', {
            params: {
                conversationId: newValue,
            }
        });
        chats.value = res.data;
    }, { immediate: true })
})
</script>