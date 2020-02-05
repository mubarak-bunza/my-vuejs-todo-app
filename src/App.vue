<template>
  <div class="container bg-light my-4">
    <app-header />
    <app-contents />
    <app-footer />
  </div>
</template>

<script>
  import Header from '@/components/Header.vue'
  import Contents from '@/components/Contents.vue'
  import Footer from '@/components/Footer.vue'

  export default {
    name: 'app',
    components: {
      'app-header': Header,
      'app-contents': Contents,
      'app-footer': Footer
    }
  }
</script>

<style lang="scss" scoped>
  

</style>
