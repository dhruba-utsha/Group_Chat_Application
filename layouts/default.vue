<template>
    <div>
      <header class="shadow-sm bg-sky-500 sticky top-0">
        <nav class="container mx-auto p-4 flex justify-between items-center text-gray-100">
          <NuxtLink to="/" class="font-bold text-[30px]">Home</NuxtLink>
          <ul class="flex gap-10">
            <li><NuxtLink to="/userPage" class="font-bold text-[30px]">User</NuxtLink></li>
            <li><NuxtLink to="/chatPage"  class="font-bold text-[30px]">Chat</NuxtLink></li>
          </ul>
        </nav>
      </header>
      <div class="container mx-auto p-4">
        <slot />
      </div>
    </div>
</template>
