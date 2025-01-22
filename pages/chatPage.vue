<template>
    <div class="flex flex-col h-screen">
        
        <div class="flex-1 overflow-y-auto p-4">
            <div v-for="(message, index) in messages" :key="index">
                
                <div v-if="message.role === 'user'" class="text-white my-4 flex justify-start">
                    <div class="bg-sky-500 p-4 rounded-md max-w-md">
                        <p class="font-bold">
                            {{ getUserName(message.id) }}:
                        </p>
                        <p>{{ message.message }}</p>
                    </div>
                </div>

                
                <div v-else-if="message.role === 'admin'" class="text-white my-4 flex justify-end">
                    <div class="bg-green-500 p-4 rounded-md max-w-md">
                        <p class="font-bold">
                            {{ getUserName(message.id) }}:
                        </p>
                        <p>{{ message.message }}</p>
                    </div>
                </div>
            </div>
        </div>

        
        <div class="flex gap-6 justify-center sticky bottom-0 bg-gray-800 p-4">
            <input
                type="text"
                placeholder="Type message..."
                v-model="newMessage.message"
                class="h-[50px] w-[400px] px-[20px] rounded-md border border-gray-300"
            />
            <select v-model="newMessage.id" class="px-[20px] h-[50px] rounded-md border border-gray-300 bg-white">
                <option value="" disabled>Select a User</option>
                <option v-for="user in users" :key="user.id" :value="user.id">
                    {{ user.role }}: {{ user.name }}
                </option>
            </select>
            <button
                @click="sendMessage"
                class="bg-sky-500 p-[0.5rem] px-[2rem] rounded-lg text-[20px] text-gray-100 font-semibold hover:bg-sky-600"
            >
                Send
            </button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            users: [], 
            messages: [], 
            newMessage: {
                message: "", 
                role: "", 
                id: "", 
            },
        };
    },
    mounted() {
        
        const storedUsers = localStorage.getItem("users");
        if (storedUsers) {
            this.users = JSON.parse(storedUsers);
        }

        
        const storedMessages = localStorage.getItem("messages");
        if (storedMessages) {
            this.messages = JSON.parse(storedMessages);
        }
    },
    methods: {
        sendMessage() {
            if (this.newMessage.message && this.newMessage.id) {
                
                const selectedUser = this.users.find((user) => user.id === this.newMessage.id);

                if (!selectedUser) return; 

                
                const newMessage = {
                    message: this.newMessage.message,
                    role: selectedUser.role.toLowerCase(), 
                    id: this.newMessage.id, 
                };

                
                this.messages.push(newMessage);

                
                localStorage.setItem("messages", JSON.stringify(this.messages));

              
                this.newMessage.message = "";
                this.newMessage.id = "";
            }
        },

        
        getUserName(userId) {
            const user = this.users.find((user) => user.id === userId);
            return user ? user.name : "Unknown User";
        },
    },
};
</script>