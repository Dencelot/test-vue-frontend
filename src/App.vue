<template>
  <div class="app-container">
    <!-- Верхние блоки -->
    <div class="top-section">
      <!-- Слева: выбранные вещи пользователя -->
      <div class="clothes">
        <h3>Выбранные вещи пользователя</h3>
        <div class="selected-items">
          <div 
            v-for="item in selectedUserItems" 
            :key="item.id" 
            class="selected-item"
          >
            {{ item.name }}
          </div>
        </div>
        <p class="info-message">
            {{ selectedUserItems.length }} / 6
          </p>
      </div>

      <!-- Справа: выбранная вещь на выбор -->
      <div class="clothes">
        <h3>Выбранная вещь на выбор</h3>
        
        <div class="selected-item-large">
          <div v-if="selectedChoiceItem" class="selected-item">
            {{ selectedChoiceItem.name }}
          </div>
          
        </div>
      </div>
    </div>

    <!-- Нижние блоки -->
    <div class="bottom-section">
      <!-- Слева: вещи пользователя -->
      <div class="bottom-col">
        <h3>Выберите 1-6</h3>
        <div class="items-grid">
          <div 
            v-for="item in userItems" 
            :key="item.id"
            :class="['item-card', { 'selected': isUserItemSelected(item.id) }]"
            @click="toggleUserItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>

      <!-- Справа: вещи на выбор -->
      <div class="bottom-col">
        <h3>Выберите 1</h3>
        <div class="items-grid">
          <div 
            v-for="item in choiceItems" 
            :key="item.id"
            :class="['item-card', { 'selected': selectedChoiceItem?.id === item.id }]"
            @click="selectChoiceItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

interface Item {
  id: number
  name: string
}

const userItems = ref<Item[]>([
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' },
  { id: 3, name: 'Item 3' },
  { id: 4, name: 'Item 4' },
  { id: 5, name: 'Item 5' },
  { id: 6, name: 'Item 6' },
])

const choiceItems = ref<Item[]>([
  { id: 101, name: 'Choice 1' },
  { id: 102, name: 'Choice 2' },
  { id: 103, name: 'Choice 3' },
])

const selectedUserItems = ref<Item[]>([])
const selectedChoiceItem = ref<Item | null>(null)

const isUserItemSelected = (id: number): boolean => {
  return selectedUserItems.value.some(item => item.id === id)
}

const toggleUserItem = (item: Item): void => {
  const index = selectedUserItems.value.findIndex(selected => selected.id === item.id)
  if (index > -1) {
    selectedUserItems.value.splice(index, 1)
  } else {
    if (selectedUserItems.value.length < 6) {
      selectedUserItems.value.push(item)
    }
  }
}

const selectChoiceItem = (item: Item): void => {
  selectedChoiceItem.value = item
}
</script>
