<template>
    <div>
        <ul v-if="allMessages.data.length > 0">
            <li v-for="m in allMessages.data">
                <strong>{{ m.message.user }}</strong>
                <div>
                    {{ m.message.text  }}
                </div>
            </li>
        </ul>
        <div v-else>
            Loading messages...
        </div>
    </div>

    <div>
        <input v-model="message" type="text" placeholder="Type your mesage here" @keyup.input.enter="sendMessage">
        <button @click="sendMessage">Send</button>
    </div>
</template>

<script setup>
    import { ref, reactive, onMounted} from 'vue';

    let user = ref("tiktokuser");

    let message = ref("");

    let allMessages = reactive({
        data: [],
    });

    function reverseMessages(){
        allMessages.data.reverse();
    }

    function sendMessage() {
        fetch('https://dev5-lab4.onrender.com/api/v1/messages', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                message:{
                    user: user.value,
                    text: message.value,
                },
            }),
        })
        .then(response => response.json())
        .then(data => {
            console.log('Success:', data);
            allMessages.data.unshift(data.data[0].message);
        })
        .catch((error) => {
            console.error('Error:', error);
        })
        
    }

    function fetchData(){
        fetch('https://dev5-lab4.onrender.com/api/v1/messages')
        .then(response => response.json())
        .then(data => {
            allMessages.data = data.data[0].messages;
            console.log(allMessages.data);
            reverseMessages();
        })
        .catch(error => {
            console.error(error);
        })
    }

    onMounted(() => {
        fetchData();

    });


</script>

<style scoped>

</style>