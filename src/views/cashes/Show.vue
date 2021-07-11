<template>
  <div class="container">
      <div class="w-full md:w-1/2">
      <div class="border rounded-lg overflow-y-hidden">
          <div class="border-b bg-gray-50 p-4">
              {{ transaction.name}}
          </div>
          <div class="p-4">
              <div class="mb-4">
                  <label class="uppercase text-gray-500 tracking-wider text-xs">Amount</label>
                  <div class="font-semibold">
                      Rp {{transaction.amount}}
                  </div>
              </div>
            <div class="mb-4">
                  <label class="uppercase text-gray-500 tracking-wider text-xs">Stored</label>
                  <div class="font-semibold">
                      Rp {{transaction.when}}
                  </div>
              </div>
            <div class="mb-4">
                  <label class="uppercase text-gray-500 tracking-wider text-xs">Description</label>
                  <div class="leading-relaxed">
                      <p>{{transaction.description}}</p>
                  </div>
              </div>
          </div>

      </div>

      </div>
  </div>
</template>

<script>
import { onMounted, ref } from '@vue/runtime-core'
import {useRoute} from 'vue-router'
import axios from 'axios'
import router from '@/router'
export default {
    setup () {
        const route = useRoute()
        const transaction = ref([])
        let slug = route.params.slug

        const getTransaction = async () => {
            try {
                let {data} = await axios.get(`api/cash/${slug}`)
                transaction.value = data.data
            } catch {
                router.replace('/cashes')
            }

        }

        onMounted(() => {
            getTransaction()
        })

        return {transaction}
    }

}
</script>