<template>
  <!-- open -->
  <div class="app-select" :class="{ open: isOpenSelect }">
    <div @click="open" class="app-select__input">
      <span>{{ select.text }}</span>
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
        v-for="(item, idx) in filteredOptions"
        :key="idx"
        class="app-select__item"
        :class="{ selected: item.selected }"
        @click="selectItem(idx)"
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
    v-if="isOpenSelect"
    @click="clickBackDrop"
    class="select-backdrop"
  ></div>
  
</template>

<script>
import { onMounted, computed, watch, reactive, ref } from 'vue'
import { useFetch } from '@/useSelect/fetch'

export default {
  props: {
    placeholder: {
      type: String
    },
    url: {
      type: String
    }
  },

  //   emits: ['open', 'clickItem'],

  setup(props, { emit }) {
    const select = reactive({
      options: [],

      isOpen: false,
      text: props.placeholder
    })

    const filter = ref('')

    let currentElementId = ref(null)

    const open = async function () {
      select.isOpen = !select.isOpen

      // console.log(await useFetch(props.url))
    
      const data = await useFetch(props.url)

      if (currentElementId.value) {
        select.options = data.map(el => {
          if (el.id === currentElementId.value) {
            return { id: el.id, value: el.name, selected: true }
          } else {
            return { id: el.id, value: el.name, selected: false }
          }
        })
      } else {
        select.options = data.map(el => {
          return { id: el.id, value: el.name, selected: false }
        })
      }
    }

    const isOpenSelect = computed(() => {
      return select.isOpen
    })

    const isResetSearchField = computed(() => {
      return filter.value.length > 0 ? true : false
    })

    const filteredOptions = computed(() => {
      return select.options.filter(el => {
        return el.value.toLowerCase().includes(filter.value.toLowerCase())
    
      })
    })

    const selectItem = function (idx) {
      const selectedItem = select.options[idx]

      select.options.forEach(el => (el.selected = false))

      selectedItem.selected = !selectedItem.selected

      if (selectedItem.selected) {
        currentElementId.value = selectedItem.id
        console.log(selectedItem.value)
      }

      select.text = selectedItem.value
      select.isOpen = false
      filter.value = ''
    }

    const reset = function () {
      select.options.forEach(el => (el.selected = false))
      select.isOpen = false
      select.text = props.placeholder
      currentElementId.value = null
      filter.value = ''
    }

    const clickBackDrop = function() {
      select.isOpen = false
      filter.value = ''
    }

    return {
      open,
      isOpenSelect,
      selectItem,
      reset,
      select,
      filteredOptions,
      filter,
      clickBackDrop,
      isResetSearchField
    }
  }
}
</script>
