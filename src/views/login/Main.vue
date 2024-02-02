<template>
    <section class="bg-gray-50 w-full h-screen flex items-center justify-center dark:bg-gray-900">
        <div class="flex w-full flex-col items-center justify-center px-6 py-8 mx-auto md:h-screen lg:py-0">
            <div
                class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
                <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                    <h1
                        class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl dark:text-white text-center">
                        Log In
                    </h1>
                    <form class="space-y-4 md:space-y-6" action="#">
                        <div>
                            <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Your
                                email</label>
                            <input type="email" v-model="email" name="email" id="email"
                                class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                placeholder="name@company.com" required="">
                        </div>
                        <div>
                            <label for="password"
                                class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Password</label>
                            <input type="password" v-model="password" name="password" id="password" placeholder="********"
                                class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                required="">
                        </div>
                        <button type="submit" @click="handleSubmit" :disabled="loading"
                            class="w-full text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">
                            <div class="loading" v-if="loading">
                                <div role="status" class="flex items-center justify-center ">
                                    <svg class="w-5 h-5 text-gray-200 animate-spin dark:text-gray-600 fill-blue-600"
                                        viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                                        <path
                                            d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                                            fill="currentColor" />
                                        <path
                                            d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                                            fill="currentFill" />
                                    </svg>
                                    <span class="sr-only">Loading...</span>
                                </div>
                            </div>
                            <span v-else>Log In</span>
                        </button>
                        <p class="text-sm font-light text-gray-500 dark:text-gray-400">
                            You have not an account? <span @click="() => { router.push('/register') }"
                                class="font-medium text-primary-600 hover:underline cursor-pointer dark:text-primary-500">Create
                                an acount</span>
                        </p>
                    </form>
                    <span class="loading loading-infinity loading-xs"></span>
                </div>
            </div>
        </div>
    </section>
</template>
<script setup>
import { ref, toRaw } from 'vue';
import axios from 'axios'
import router from '@/router';
import Swal from 'sweetalert2'

const loading = ref(false)
const email = ref('');
const password = ref('');

const handleSubmit = async (e) => {
    e.preventDefault();
    loading.value = true;
    const data = {
        email: email.value,
        password: password.value,
    };
    if (data.email === '' || data.password === '') {
        // Swal.fire({
        //     position: "top-center",
        //     icon: "error",
        //     title: "Eror",
        //     showConfirmButton: false,
        //     timer: 1500
        // });
        loading.value = false
    } else {
        try {
            await axios.post('https://ho-tel.1kb.uz/api/Admin/login-admin', data)
                .then(function (response) {
                    console.log(response);
                    if (response.status === 200) {
                        router.push('/mainpage');
                        localStorage.setItem("token", response.data.token);
                    } else if (response.status !== 200) {
                        alert("Xatolik qayta tekshiring")
                    }
                }).catch((err) => {
                    console.log(err,955);
                });
        } finally {
            loading.value = false;
        }
    }
    console.log(toRaw(data));
};
// loading.value = false;

</script>
