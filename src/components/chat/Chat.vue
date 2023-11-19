<template>
    <div>
        <ul >
            <li v-for="m in allMessages.data">{{ m }}</li>
        </ul>
    </div>

    <div>
        <input v-model="message" type="text" placeholder="Type your mesage here" @keyup.input.enter="sendMessage">
        <button @click="sendMessage">Send</button>
    </div>
</template>

<script setup>
    import { ref, reactive, onMounted} from 'vue';

    let message = ref("");
    let allMessages = reactive({
        data: ["hey", "hello", "hi", "sup"],
    });

    function sendMessage() {
        if(message.value !== ""){
            allMessages.data.push(message.value);
        }
    }

</script>

<style scoped>

</style>