<template>
  <div id="home">
    <app-select
      placeholder="Choose options..."
      :data="users"
      param="name"
    ></app-select>
  </div>
</template>

<script>
import AppSelect from '@/components/AppSelect.vue'
import { ref, reactive, computed } from 'vue'

export default {
  setup() {

    const users = 'https://jsonplaceholder.typicode.com/users'

    const placeholder = 'Open this select menu'
    
    return {
      users
    }
  },

  components: {
    AppSelect
  }
}
</script>
