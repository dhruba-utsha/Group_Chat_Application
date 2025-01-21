<template>
    <h1 class="text-3xl font-bold text-green-500 underline">
      Hello This is home page.
    </h1>
    <div class="flex justify-between items-center py-[5rem]">
        <div>
            <h1 class="text-[50px] font-bold"> Let's Connect With <br> Group Chat Application  </h1>

            <NuxtLink to="/userPage">
              <button class="bg-sky-500 p-[0.5rem] px-[1rem] mt-[2rem] rounded-md text-[20px] text-gray-100 font-semibold"> Get Started </button>
            </NuxtLink>
        </div>

        <div>
            <img src="assets\images\group_image.jpg" alt="banner-image" width="[100%]">
        </div>
    </div>
</template>