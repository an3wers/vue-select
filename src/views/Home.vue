<template>
  <div id="home">
    <app-select
      placeholder="Выбрать пользователя..."
      :data="users"
      param="name"
    ></app-select>
    <br>
    <app-select
      placeholder="Выбрать email..."
      :data="users"
      param="email"
    ></app-select>
  </div>
</template>

<script>
import AppSelect from '@/components/AppSelect.vue'
import { ref, reactive, computed } from 'vue'

export default {
  setup() {

    const users = 'https://jsonplaceholder.typicode.com/users'

    
    return {
      users
    }
  },

  components: {
    AppSelect
  }
}
</script>
