<template>
    <div class="flex flex-col h-[600px]">
        
        <div ref="messagesContainer" class="flex-1 overflow-y-scroll mb-5 border-[3px] border-sky-500 px-[10px]">
            <div v-for="(message, index) in messages" :key="index">
            
                <div v-if="message.role === 'member'" class="text-white my-4 flex justify-start">
                    <div class="bg-gray-300 px-4 py-2 rounded-xl w-[40%]">
                        <p class="font-bold text-[20px] py-3 text-sky-700">
                            <span class="text-white bg-gray-400 p-3 rounded-[50%]">{{ getFirstLetter(getUserName(message.id)) }}</span>
                            {{ getUserName(message.id) }}
                            <span class="text-gray-600 pl-[18rem] text-[12px]">{{ formatDate(message.date) }}</span>
                        </p>
                        <p class="text-[18px] text-black pl-12 pb-3">{{ message.message }}</p>
                    </div>
                </div>

                
                <div v-else-if="message.role === 'admin'" class="text-white my-4 flex justify-end">
                    <div class="bg-gray-300 px-4 py-2 rounded-xl w-[40%]">
                        <p class="font-bold text-[20px] py-3 text-green-700">
                            <span class="text-white bg-gray-400 p-3 rounded-[50%]">{{ getFirstLetter(getUserName(message.id)) }}</span>
                            {{ getUserName(message.id) }}
                            <span class="text-gray-600 pl-[18rem] text-[12px]">{{ formatDate(message.date) }}</span>
                        </p>
                        <p class="text-[18px] text-black pl-12 pb-3">{{ message.message }}</p>
                    </div>
                </div>
            </div>
        </div>

        
        <div @keydown.enter="sendMessage" class="flex gap-6 justify-center bg-sky-500 p-4 rounded-lg">
            <input
                type="text"
                placeholder="Type message..."
                v-model="newMessage.message"
                class="h-[50px] w-[400px] px-[20px] rounded-md border border-blue-800"
            />
            <select v-model="newMessage.id" class="px-[20px] h-[50px] rounded-md border border-blue-800 bg-white">
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
});


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
    if (newMessage.value.message && newMessage.value.id) {
        const selectedUser = users.value.find((user) => user.id === newMessage.value.id);
        if (!selectedUser) return;

        const newMsg = {
            message: newMessage.value.message,
            role: selectedUser.role.toLowerCase(),
            id: newMessage.value.id,
            date: new Date().toISOString(),
        };

       
        messages.value.push(newMsg);
        localStorage.setItem("messages", JSON.stringify(messages.value));

        
        newMessage.value.message = "";
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
    const container = document.querySelector('[ref="messagesContainer"]');
    if (container) {
        container.scrollTop = container.scrollHeight;
    }
};
</script>
