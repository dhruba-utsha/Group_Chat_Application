<template>
    <div class="flex gap-6 justify-center ">
        <input type="text" placeholder="Type message..." v-model="newMessage.message"
            class="h-[50px] w-[400px] px-[20px]" />
        <select v-model="newMessage.role" class="px-[20px]">
            <option value="" disabled>Select a User</option>
            <option value="admin">Admin</option>
            <option value="user">User</option>
        </select>
        <button @click="sendMessage"
            class="bg-sky-500 p-[0.5rem] px-[2rem] rounded-lg text-[20px] text-gray-100 font-semibold">
            Send
        </button>
    </div>

    <div  v-if="messages.length > 0 && messages[messages.length - 1].role === 'admin'">
        <div class="text-white py-4 grid justify-end">
            <ul class="space-y-4">
                <li class="py-5 px-10 bg-sky-500 rounded-md">
                    <span>
                        <p>Message: {{ messages[messages.length - 1].message }}</p>
                        <p>Role: {{ messages[messages.length - 1].role}}</p>
                    </span>
                </li>
            </ul>
        </div>
    </div>

    <div  v-if="messages.length > 0 && messages[messages.length - 1].role === 'user'">
        <div class="text-white py-4 grid justify-start">
            <ul class="space-y-4">
                <li class="py-5 px-10 bg-sky-500 rounded-md">
                    <span>
                        <p>Message: {{ messages[messages.length - 1].message }}</p>
                        <p>Role: {{ messages[messages.length - 1].role}}</p>
                    </span>
                </li>
            </ul>
        </div>
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

    recent(){
        const last = messages[messages.length()-1]
        return last
    },
};

</script>