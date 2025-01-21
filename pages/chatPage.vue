<template>
    <div class="flex gap-6 justify-center ">
        <input type="text" placeholder="Type message..." v-model="newMessage.message"
            class="h-[50px] w-[400px] px-[20px]" />
        <select v-model="newMessage.role" class="px-[20px]">
            <option value="">Select a User</option>
            <option>Admin</option>
            <option>User</option>
        </select>
        <button @click="sendMessage"
            class="bg-sky-500 p-[0.5rem] px-[2rem] rounded-lg text-[20px] text-gray-100 font-semibold">
            Send
        </button>
    </div>

    <div class="bg-white text-white py-4">
        <ul class="space-y-4">
            <li v-for="(message, index) in messages" :key="index"
                class="p-5 bg-sky-500 rounded-md flex justify-between items-center">
                <span>
                    <p>Message: {{ message.message }}</p>
                    <p>Role: {{ message.role }}</p>
                </span>
                
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            messages: [],  
            newMessage: {
                message: "",
                role: "",
            },
        };
    },
    mounted() {
       
        const storedMessages = localStorage.getItem("messages");
        if (storedMessages) {
            this.messages = JSON.parse(storedMessages);
        }
    },
    methods: {
        sendMessage() {
            if (this.newMessage.message && this.newMessage.role) {
                const newMessage = {
                    message: this.newMessage.message,
                    role: this.newMessage.role,
                };

                this.messages.push(newMessage);

                
                localStorage.setItem("messages", JSON.stringify(this.messages));

                
                this.newMessage.message = "";
                this.newMessage.role = "";
            }
        },
       
    },
};
</script>

