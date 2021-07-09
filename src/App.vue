<template>
  <Navbar/>
  <div class="mt-8">
    <router-view/>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar'
export default {
  components: {Navbar},

}
</script>

<style>

</style>