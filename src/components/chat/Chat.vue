<template>
    <div>
        <ul >
            <li v-for="m in allMessages.data">
                <strong>{{ m.message.user }}</strong>
                <div>
                    {{ m.message.text  }}
                </div>
            </li>

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
        data: [],
    });

    function sendMessage() {
        fetch('https://dev5-lab4.onrender.com/api/v1/messages', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                message:{
                    user: 'tiktokuser',
                    text: message.value,
                }
            })
        })
    }

    onMounted(() => {
        fetch('https://dev5-lab4.onrender.com/api/v1/messages')
        .then(response => response.json())
        .then(data => {
            allMessages.data = data.data[0].messages;
            console.log(allMessages.data);
        })
        .catch(error => {
            console.error(error);
        })

    });


</script>

<style scoped>

</style>