<template>
  <div id="home">
    <app-select
      :placeholder="placeholder"
      :url="URL_DATA"
    ></app-select>
  </div>
</template>

<script>
import AppSelect from '@/components/AppSelect.vue'
import { ref, reactive, computed } from 'vue'

export default {
  setup() {

    const URL_DATA = 'https://jsonplaceholder.typicode.com/users'

    const placeholder = 'Open this select menu'
    
    return {
      placeholder,
      URL_DATA
    }
  },

  components: {
    AppSelect
  }
}
</script>
