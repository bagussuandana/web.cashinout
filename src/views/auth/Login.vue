<template>
  <div class="container">
    <div class="border rounded-lg w-full md:w-1/3 overflow-hidden">
      <div class="bg-gray-50 p-4 border-b">
        Login
      </div>
      <div class="p-4">
        <form @submit.prevent="login">
          <div class="mb-5">
            <label for="email" class="text-xs uppercase font-medium">Email</label>
            <input type="email" id="email" name="email" v-model="credential.email" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <div class="text-red-500 mt-2 text-xs" v-if="errors['email']">{{errors['email'][0]}}</div>
          </div>
          <div class="mb-5">
            <label for="password" class="text-xs uppercase font-medium">Password</label>
            <input type="password" id="password" name="password" v-model="credential.password" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <div class="text-red-500 mt-2 text-xs" v-if="errors['password']">{{errors['password'][0]}}</div>
          </div>
          <div>
            <button class="bg-blue-500 rounded-lg text-white text-center px-4 py-2 hover:bg-blue-600 transition duration-200">
              Login
            </button>
          </div>
        </form>

      </div>
      
    </div>
  </div>
</template>

<script>
import store from '@/store'
import router from '@/router'
import { reactive, ref } from '@vue/reactivity';

export default {
  setup () {
    const errors = ref([])
    const credential = reactive({
      email: '',
      password: '',
    })
    const login = async () => {
      try {
          await store.dispatch("auth/login", credential)
          router.replace("/")
          } catch (e) {
            errors.value = e.response.data.errors;
          }


    }

    return {login, credential, errors}
  }

};
</script>

<style>

</style>