<template>
    <div class="flex flex-col h-[600px]">

        <div class="flex-1 overflow-y-scroll mb-5 border-[3px] border-sky-500 px-[10px]">
            <div v-for="(message, index) in messages" :key="index">

                <div v-if="message.role === 'member'" class="text-white my-4 flex justify-start">
                    <div class="bg-gray-300 px-4 py-2 rounded-xl w-[40%]">
                        <p class="font-bold text-[20px] text-sky-700">
                            {{ getUserName(message.id) }}
                        </p>
                        <p class="text-[18px] text-black">{{ message.message }}</p>
                    </div>
                </div>

                <div v-else-if="message.role === 'admin'" class="text-white my-4 flex justify-end">
                    <div class="bg-gray-300 px-4 py-2 rounded-xl w-[40%]">
                        <p class="font-bold text-[20px] text-green-700">
                            {{ getUserName(message.id) }}
                        </p>
                        <p class="text-[18px] text-black">{{ message.message }}</p>
                    </div>
                </div>
            </div>
        </div>


        <div @keydown.enter="sendMessage" class="flex gap-6 justify-center bg-sky-500 p-4 rounded-lg">
            <input type="text" placeholder="Type message..." v-model="newMessage.message"
                class="h-[50px] w-[400px] px-[20px] rounded-md border border-blue-800" />
            <select v-model="newMessage.id" class="px-[20px] h-[50px] rounded-md border border-blue-800 bg-white">
                <option value="" disabled>Select a Role</option>
                <option v-for="user in users" :key="user.id" :value="user.id">
                    {{ user.role }}: {{ user.name }}
                </option>
            </select>
            <button @click="sendMessage"
                class="bg-blue-800 p-[0.5rem] px-[2rem] rounded-lg text-[20px] text-gray-100 font-semibold hover:bg-blue-600">
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