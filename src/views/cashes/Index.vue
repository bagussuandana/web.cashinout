<template>
  <div class="container">
    <div class="flex">
      <!-- Data -->
        <div class="w-full md:w-8/12">
          <div class="w-full mb-8">
              <div class="bg-gray-200 transform -rotate-2 rounded-2xl">
                <div class="bg-gradient-to-br from-blue-500 to-lightBlue-400 text-white transform rotate-2 p-6 rounded-2xl">
                  <label class="block uppercase text-xs text-blue-100 font-semibold tracking-wider mb-1">balances</label>
                  <div class="text-3xl font-semibold">
                    Rp {{ state.balances }}
                  </div>
                </div>
              </div>
          </div>
          <div class="flex items-center -mx-2">
            <div class="w-full px-2">
              <div class="bg-gray-200 transform -rotate-3 rounded-2xl">
                <div class="bg-gradient-to-br from-teal-500 to-cyan-400 text-white transform rotate-3 p-6 rounded-2xl">
                  <label class="block uppercase text-xs text-teal-100 font-semibold tracking-wider mb-1">debit</label>
                  <div class="text-3xl font-semibold">
                    Rp {{state.debit}}
                  </div>
                </div>
              </div>

            </div>
            <div class="w-full px-2">
              <div class="bg-gray-200 transform -rotate-3 rounded-2xl">
                <div class="bg-gradient-to-br from-red-500 to-yellow-400 text-white transform rotate-3 p-6 rounded-2xl">
                  <label class="block uppercase text-xs text-red-100 font-semibold tracking-wider mb-1">credit</label>
                  <div class="text-3xl font-semibold">
                    Rp {{state.credit}}
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="mt-10 w-full">
          <div class="border rounded-lg overflow-hidden">
            <div class="border-b px-6 py-4 bg-gray-50 flex items-center justify-between">
              <div>
                Transaction
              </div>
              <div>
                <form @submit.prevent="getCashes" class="flex items-center">
                    <input type="date" v-model="form.begin" class="bg-white border rounded px-3 py-2 mr-2">
                    <input type="date" v-model="form.to" class="bg-white border rounded px-3 py-2 mr-2">
                    <input type="submit" value="Go" class="px-3 py-2 rounded bg-gradient-to-br from-blue-500 to-lightBlue-500 text-white focus:outline-none">
                </form>
              </div>

            </div>
            <div class="px-6 py-4 h-112 overflow-y-auto">
              <template v-for="transaction in state.transactions" :key="transaction.id">
                <router-link :to="`/cashes/${transaction.slug}`" class="flex justify-between items-center hover:bg-gray-50 p-4 rounded-lg hover:shadow mb-2">
                  <span class="flex flex-col">
                    <span class="text-gray-500 text-xs">{{ transaction.when}}</span>
                    <span>{{ transaction.name}}</span>
                  </span>
                  <span :class="transaction.isCredit ? 'text-red-500' : 'text-green-500'">{{ transaction.amount}}</span>
                </router-link>
              </template>



            </div>

          </div>

        </div>

        </div>

        
      <!-- End Data -->
      <!-- Input -->
      <div class="w-full md:w-4/12 p-6">
        <h1 class="font-semibold text-lg text-gray-800 mb-8">Add Transactions History</h1>
        <form @submit.prevent="add">
          <div class="mb-5">
            <label for="name" class="text-xs uppercase font-medium">Name</label>
            <input type="text" id="name" name="name" v-model="form.name" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <!-- <div class="text-red-500 mt-2 text-xs" v-if="errors['name']">{{errors['name'][0]}}</div> -->
          </div>
          <div class="mb-5">
            <label for="amount" class="text-xs uppercase font-medium">Amount</label>
            <input type="text" id="amount" name="amount" v-model="form.amount" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <!-- <div class="text-red-500 mt-2 text-xs" v-if="errors['amount']">{{errors['amount'][0]}}</div> -->
          </div>
          <div class="mb-5">
            <label for="when" class="text-xs uppercase font-medium">When</label>
            <input type="date" id="when" name="when" v-model="form.when" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 h-10 px-4 transition duration-200">
            <!-- <div class="text-red-500 mt-2 text-xs" v-if="errors['when']">{{errors['when'][0]}}</div> -->
          </div>
          <div class="mb-5">
            <label for="description" class="text-xs uppercase font-medium">Description</label>
            <textarea type="text" id="description" name="description" v-model="form.description" class="w-full border rounded-lg focus:outline-none focus:ring focus:border-blue-500 py-4 px-4 transition duration-200"></textarea>
            <!-- <div class="text-red-500 mt-2 text-xs" v-if="errors['description']">{{errors['description'][0]}}</div> -->
          </div>
          <div>
            <button class="bg-blue-500 rounded-lg text-white text-center px-4 py-2 hover:bg-blue-600 transition duration-200">
              Add Transaction
            </button>
          </div>
        </form>
      

      </div>
      <!-- End Input -->

    </div>
      

  </div>
</template>

<script>
import { reactive, ref } from '@vue/reactivity'
import axios from 'axios'
import { onMounted } from '@vue/runtime-core'
export default {
  setup () {
    const state = ref([])

    const form = reactive({
      begin: '',
      to: '',
      name: '',
      amount: '',
      when: '',
      description:'',
    })
    const getCashes = async () => {
      let {data} = await axios.get('api/cash', {
        params: {
          from: form.begin,
          to: form.to,
        }

      });
      state.value = data
      form.begin = data.firstOfMonth
      form.to = data.now
    }
    const add = async () => {
      let response = await axios.post('api/cash/create', form)
      state.value.transaction.unshift(response.data.cash)
    }

    onMounted(() => {
      getCashes()
    })

    return {state, form, getCashes, add}
  }

}
</script>

<style>

</style>