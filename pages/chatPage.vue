<template>
    <div class="flex flex-col h-[700px]">
        
        <div ref="messagesContainer" class="flex-1 overflow-auto mb-5 border-[3px] border-sky-500 px-[10px]">
            <div v-for="(message, index) in messages" :key="index">
            
                <div v-if="message.role === 'member'" class="text-white my-4 flex justify-start">
                    <div class="bg-gray-300 px-4 py-2 rounded-xl w-[40%]">
                        <div class="font-bold text-[20px] py-3 text-sky-700 flex items-center justify-between">
                            <p>
                            <span class="text-white bg-gray-600 p-3 rounded-[50%]">{{ getFirstLetter(getUserName(message.id)) }}</span>
                            {{ getUserName(message.id) }}
                            </p>
                            <p class="text-gray-600 text-[12px]">{{ formatDate(message.date) }}</p>
                        </div>
                        <p v-if="message.message" class="text-[18px] text-black pl-12 pb-3">{{ message.message }}</p>
                        <img v-if="message.image" :src="message.image" alt="User Uploaded Image" class="mx-auto py-3" />
                    </div>
                </div>
                
                <div v-else-if="message.role === 'admin'" class="text-white my-4 flex justify-end">
                    <div class="bg-gray-300 px-4 py-2 rounded-xl w-[40%]">
                        <div class="font-bold text-[20px] py-3 text-green-700 flex items-center justify-between">
                            <p>
                            <span class="text-white bg-gray-600 p-3 rounded-[50%]">{{ getFirstLetter(getUserName(message.id)) }}</span>
                            {{ getUserName(message.id) }}
                            </p>
                            <p class="text-gray-600 pl-[10rem] text-[12px]">{{ formatDate(message.date) }}</p>
                        </div>
                        <p class="text-[18px] text-black pl-12 pb-3">{{ message.message }}</p>
                        <img v-if="message.image" :src= "message.image" alt="User Uploaded Image" class="mx-auto py-3" />
                    </div>
                </div>
            </div>
        </div>
        
        <div class="flex gap-6 justify-center bg-sky-500 p-4 rounded-lg">
            <input type="text" placeholder="Enter Image link..." v-model="newMessage.image" class="h-[50px] w-[200px] px-[20px] rounded-md border border-blue-800">
            <input
                @keydown.enter.prevent="sendMessage"
                type="text"
                placeholder="Type message..."
                v-model="newMessage.message"
                class="h-[50px] w-[400px] px-[20px] rounded-md border border-blue-800"
            />
            <select @keydown.enter.prevent="sendMessage" v-model="newMessage.id" class="px-[20px] h-[50px] rounded-md border border-blue-800 bg-white" required>
                <option value="" disabled>Select a Role</option>
                <option v-for="user in users" :key="user.id" :value="user.id">
                    {{ user.role }}: {{ user.name }}
                </option>
            </select>
            <button
                @click="sendMessage"
                class="bg-blue-800 p-[0.5rem] px-[2rem] rounded-lg text-[20px] text-gray-100 font-semibold hover:bg-blue-600"
            >
                Send
            </button>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, nextTick } from "vue";


const users = ref([]);
const messages = ref([]);
const newMessage = ref({
    message: "",
    id: "",
    image: ""
});

const messagesContainer = ref(null);

onMounted(() => {
    const storedUsers = localStorage.getItem("users");
    if (storedUsers) {
        users.value = JSON.parse(storedUsers);
    }

    const storedMessages = localStorage.getItem("messages");
    if (storedMessages) {
        messages.value = JSON.parse(storedMessages);
    }

    scrollToBottom();
});


const sendMessage = () => {
    if (newMessage.value.message || newMessage.value.id || newMessage.value.image) {
        const selectedUser = users.value.find((user) => user.id === newMessage.value.id);
        if (!selectedUser) return;

        const newMsg = {
            message: newMessage.value.message,
            image: newMessage.value.image,
            role: selectedUser.role.toLowerCase(),
            id: newMessage.value.id,
            date: new Date().toISOString(),
        };

       
        messages.value.push(newMsg);
        localStorage.setItem("messages", JSON.stringify(messages.value));

        
        newMessage.value.message = "";
        newMessage.value.image = "";
        newMessage.value.id = "";

        
        nextTick(() => {
            scrollToBottom();
        });
    }
};

const getUserName = (userId) => {
    const user = users.value.find((user) => user.id === userId);
    return user ? user.name : "Unknown User";
};

const getFirstLetter = (userId) => {
    const user = users.value.find((user) => user.name === userId);
    return user ? userId.slice(0, 2).toUpperCase() : "--";
};

const formatDate = (dateString) => {
    const date = new Date(dateString);
    return date.toLocaleString();
};

const scrollToBottom = () => {
    if (messagesContainer.value) {
        messagesContainer.value.scrollTop = messagesContainer.value.scrollHeight;
    }
};

</script>