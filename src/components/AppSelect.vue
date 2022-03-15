<template>
  <!-- open -->
  <div class="app-select" :class="{ open: selectorIsOpen }">
    <div @click="open" class="app-select__input">
      <span v-if="selValue">{{ selValue }}</span>
      <span v-else>{{ placeholder }}</span>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="16"
        fill="currentColor"
        class="bi bi-chevron-down"
        viewBox="0 0 16 16"
      >
        <path
          fill-rule="evenodd"
          d="M1.646 4.646a.5.5 0 0 1 .708 0L8 10.293l5.646-5.647a.5.5 0 0 1 .708.708l-6 6a.5.5 0 0 1-.708 0l-6-6a.5.5 0 0 1 0-.708z"
        />
      </svg>
    </div>

    <div class="app-select__modal">
      <div class="control-select-modal">
        <input type="text" placeholder="Поиск..." v-model="filter">
        <span v-if="isResetSearchField" @click="filter = ''" class="icon-close"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x" viewBox="0 0 16 16">
  <path d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"/>
</svg></span>
      </div>
      <div class="app-select__body">
        <div
        v-if="placeholder"
        @click="reset"
        class="app-select__item app-select__item_default"
      >
        {{ placeholder }}
      </div>
        <div
          v-for="item in filteredOptions"
          :key="item.id"
          class="app-select__item"
          :class="{ selected: item.selected }"
          @click="selectItem(item.id)"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-check-lg select-item-icon"
            viewBox="0 0 16 16"
            v-if="item.selected"
          >
            <path
              d="M12.736 3.97a.733.733 0 0 1 1.047 0c.286.289.29.756.01 1.05L7.88 12.01a.733.733 0 0 1-1.065.02L3.217 8.384a.757.757 0 0 1 0-1.06.733.733 0 0 1 1.047 0l3.052 3.093 5.4-6.425a.247.247 0 0 1 .02-.022Z"
            />
          </svg>
          <span>{{ item.value }}</span>
        </div>
      </div>
    </div>
  </div>
  <div
    v-if="selectorIsOpen"
    @click="clickBackDrop"
    class="select-backdrop"
  ></div>
  <p>{{ currentElement }}</p>
</template>

<script>
import { onMounted, computed, watch, reactive, ref } from 'vue'
import { useFetch } from '@/fetch'

export default {
  props: ['data', 'placeholder', 'param'],

  setup({ data, param, placeholder }, { emit }) {
    const options = ref([])
    let selValue = ref(null)
    let currentElementId = ref(null)
    let currentElement = ref(null)
    const isOpen = ref(false)
    const loading = ref(false)
    const filter = ref('')

    const open = async () => {
      isOpen.value = !isOpen.value

      loading.value = true
      const result = await useFetch(data)
      loading.value = false

      if (currentElementId.value) {
        options.value = result.map(el => {
          if (el.id === currentElementId.value) {
            return { id: el.id, value: el[param], selected: true }
          } else {
            return { id: el.id, value: el[param], selected: false }
          }
        })
      } else {
        options.value = result.map(el => {
          return { id: el.id, value: el[param], selected: false }
        })
      }
    }

    const selectItem = id => {
      options.value.forEach(el => {
        return (el.selected = false)
      })
      const selectedItem = options.value.find(el => el.id === id)
      selectedItem.selected = !selectedItem.selected

      if (selectedItem.selected) {
        currentElementId.value = selectedItem.id
        currentElement.value = selectedItem
        selValue.value = selectedItem.value
      }
      isOpen.value = false
      filter.value = ''
    }

    const selectorIsOpen = computed(() => isOpen.value)

    const isResetSearchField = computed(() => {
      return filter.value.length > 0 ? true : false
    })

    const filteredOptions = computed(() => {
      return options.value.filter(el => {
        return el.value.toLowerCase().includes(filter.value.toLowerCase())
      })
    })

    const clickBackDrop = () => {
      isOpen.value = false
      filter.value = ''

    }

    const reset = () => {
      options.value.forEach(el => el.selected = false)
      isOpen.value = false
      selValue.value = null
      currentElementId.value = null
      currentElement.value = null
      filter.value = ''
    }

    return {
      open,
      selValue,
      selectorIsOpen,
      clickBackDrop,
      selectItem,
      currentElement,
      reset,
      isResetSearchField,
      filteredOptions,
      filter
    }
  }
}
</script>
