<template>
  <div class="border-b">
      <div class="flex flex-col md:flex-row py-4">
          <div class="flex justify-between items-center px-6">
              <div class="uppercase font-semibold">
                <router-link :class="className" to="/" exact-active-class="bg-transparent">
                Cashinout
                </router-link>
              </div>

              <button @click="isOn = !isOn" class="block md:hidden focus:outline-none">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path :class="!isOn ? 'block' : 'hidden'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
                    <path :class="isOn ? 'block' : 'hidden'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                </svg>
              </button>
          </div>
          <div :class="isOn ? 'block': 'hidden'" class="md:flex flex-col md:flex-row justify-between md:items-center w-full px-6 py-4 md:py-0">
            <div class="flex flex-col md:flex-row md:items-center">
                <router-link :class="className" to="/about">About</router-link>
                <router-link :class="className" to="/cashes">Cash</router-link>
            </div>
            <div class="flex flex-col md:flex-row md:items-center" v-if="authenticated">
                <router-link :class="className" to="">{{user.name}}</router-link>
                <button :class="className" @click="logout">Logout</button>
            </div>
            <div class="flex flex-col md:flex-row md:items-center" v-else>
                <router-link :class="className" to="/login">Login</router-link>
                <router-link :class="className" to="/register">Register</router-link>
            </div>
          </div>

      </div>

  </div>
</template>

<script>
import { ref } from '@vue/reactivity'
import store from '@/store'
import { computed } from '@vue/runtime-core'
export default {
    setup () {
        const className = "px-4 py-2"
        const isOn = ref(false)

        const authenticated = computed(() => store.getters['auth/authenticated'])
        const user = computed(() => store.getters['auth/user'])

        const logout = async () => {
          store.dispatch("auth/logout")

        }

        return {className , isOn ,user , authenticated, logout}
    }

}
</script>

<style>

</style>