<template>
  <div class="container">
    <div class="border rounded-lg w-full md:w-1/3 overflow-hidden">
      <div class="bg-gray-50 p-4 border-b">
        Register
      </div>
      <div class="p-4">
        <form @submit.prevent="register">
          <div class="mb-5">
            <label for="name" class="text-xs uppercase font-medium">Name</label>
            <input type="text" id="name" name="name" v-model="form.name" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <div class="text-red-500 mt-2 text-xs" v-if="errors['name']">{{errors['name'][0]}}</div>
          </div>
          <div class="mb-5">
            <label for="email" class="text-xs uppercase font-medium">Email</label>
            <input type="email" id="email" name="email" v-model="form.email" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <div class="text-red-500 mt-2 text-xs" v-if="errors['email']">{{errors['email'][0]}}</div>
          </div>
          <div class="mb-5">
            <label for="password" class="text-xs uppercase font-medium">Password</label>
            <input type="password" id="password" name="password" v-model="form.password" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <div class="text-red-500 mt-2 text-xs" v-if="errors['password']">{{errors['password'][0]}}</div>
          </div>
          <div class="mb-5">
            <label for="password_confirmation" class="text-xs uppercase font-medium">Password Confirmation</label>
            <input type="password" id="password_confirmation" name="password_confirmation" v-model="form.password_confirmation" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
          </div>
          <div>
            <button class="bg-blue-500 rounded-lg text-white text-center px-4 py-2 hover:bg-blue-600 transition duration-200">
              Register
            </button>
          </div>
        </form>

      </div>
      
    </div>
  </div>
</template>

<script>
import { reactive, ref } from '@vue/reactivity';
import axios from 'axios'
import router from '@/router'
import store from '@/store'

export default {
  setup () {
    const errors = ref([])
    const form = reactive({
      name: '',
      email: '',
      password: '',
      password_confirmation: '',
    })

    const register = async () => {
      try {
        await axios.post('register', form)
        await store.dispatch('auth/me')
        router.replace('/')
      } catch (e) {
        errors.value = e.response.data.errors;
      }

    }

    return {form, register, errors}
  }

};
</script>

<style>

</style>