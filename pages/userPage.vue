<template>
  <div class="flex flex-row">
   
    <div class="bg-white text-white p-4 flex-1 w-[55%] h-[600px] overflow-scroll">
      <h1 class="font-bold mb-4 text-center text-[30px] text-black">User List</h1>
      <ul class="space-y-4">
        <li v-for="user in users" :key="user.id" class="p-5 bg-sky-500 rounded-md flex justify-between items-center">
          <div>
            <p>Name: {{ user.name }}</p>
            <p>Email: {{ user.email }}</p>
            <p>Role: {{ user.role }}</p>
          </div>
          <button @click="deleteUser(user.id)"
            class="bg-red-500 hover:bg-red-700 text-white px-3 py-1 rounded-md flex items-center justify-center"
            aria-label="Delete">
            &times;
          </button>
        </li>
      </ul>
    </div>

    
    <div class="w-[45%] h-1/2 rounded-md pt-[7%] pl-[5%]">
      <div class="bg-sky-500 px-[100px] py-10 rounded">
        <h2 class="text-xl font-bold text-white mb-4 text-center">Add New User</h2>
        <form @submit.prevent="addUser" class="space-y-4">
          <div>
            <label class="block text-white mb-1" for="name">Name</label>
            <input id="name" v-model="newUser.name" type="text" class="w-full p-2 rounded-md border border-gray-300"
              placeholder="Enter name" required />
          </div>
          <div>
            <label class="block text-white mb-1" for="email">Email</label>
            <input id="email" v-model="newUser.email" type="email" class="w-full p-2 rounded-md border border-gray-300"
              placeholder="Enter email" required />
          </div>
          <div>
            <label class="block text-white mb-1" for="user-role">Role</label>
            <select id="user-role" v-model="newUser.role"
              class="w-full p-2 rounded-md border border-gray-300 bg-white mb-2" required>
              <option value="" disabled>Select a role</option>
              <option value="Admin">Admin</option>
              <option value="Member">Member</option>
            </select>
          </div>
          <div class="flex justify-center">
            <button @click="handleAlert" type="submit"
              class="px-4 py-2 bg-blue-800 text-white rounded-md hover:bg-slate-900">
              Add User
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import Swal from "sweetalert2";


const users = useState("users", () => {
  const storedUsers = localStorage.getItem("users");
  return storedUsers ? JSON.parse(storedUsers) : [];
});


const newUser = ref({
  name: "",
  email: "",
  role: "",
});


watch(
  users,
  (newUsers) => {
    localStorage.setItem("users", JSON.stringify(newUsers));
  },
  { deep: true }
);


const handleAlert = () => {
  if (newUser.value.name && newUser.value.email && newUser.value.role) {
    if (
      newUser.value.role === "Admin" &&
      users.value.some((user) => user.role === "Admin")
    ) {
      Swal.fire({
        title: "Admin Already Added!",
        icon: "error",
      });
    } else {
      Swal.fire({
        title: "User Successfully Added!",
        icon: "success",
      });
    }
  }
};


const addUser = () => {
  if (
    newUser.value.role === "Admin" &&
    users.value.some((user) => user.role === "Admin")
  ) {
    return;
  }

  if (newUser.value.name && newUser.value.email && newUser.value.role) {
    const newUserData = {
      id: Date.now(),
      name: newUser.value.name,
      email: newUser.value.email,
      role: newUser.value.role,
    };

    users.value.push(newUserData);

    
    newUser.value.name = "";
    newUser.value.email = "";
    newUser.value.role = "";

    
    Swal.fire({
      title: "User Successfully Added!",
      icon: "success",
    });
  }
};


const deleteUser = (userId) => {
  users.value = users.value.filter((user) => user.id !== userId);

  
  Swal.fire({
    title: "User Deleted!",
    icon: "success",
  });
};
</script>