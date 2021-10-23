<template>
    <div class="flex flex-col  items-center mx-auto border-4 p-3 w-8/12 h-auto">
      
      <AutoComplete
      :items="options"
       />
     
  </div>
</template>

<script>
import { ref } from '@vue/reactivity';
   import AutoComplete from './components/AutoComplete.vue';
  export default {
    components:{
      AutoComplete,
    },
    setup () {
      const options=ref([
        'Apple',
        'Banana',
        'Orange',
        'Mango',
        'Pear',
        'Peach',
        'Grape',
        'Tangerine',
        'Pineapple'
      ])
    return {
      options
    }
    }
  }
</script>

<style lang="scss" scoped>

</style>