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
        <p v-if="!selectedChoiceItem" class="info-message">
            Нет выбранной вещи
        </p>
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
import { ref, computed } from 'vue'

interface Item {
  id: number
  name: string
}

// Данные для нижнего блока слева
const userItems = ref<Item[]>([
  { id: 1, name: "Shoes 1" },
  { id: 2, name: "Shoes 2" },
  { id: 3, name: "Shoes 3" },
  { id: 4, name: "Shoes 4" },
  { id: 5, name: "T-shirt 1" },
  { id: 6, name: "T-shirt 2" },
  { id: 7, name: "T-shirt 3" },
  { id: 8, name: "T-shirt 4" }
])

// Данные для нижнего блока справа
const choiceItems = ref<Item[]>([
  { id: 11, name: "Jacket 1" },
  { id: 12, name: "Jacket 2" },
  { id: 13, name: "Jacket 3" },
  { id: 14, name: "Jacket 4" },
  { id: 15, name: "Hoodie 1" },
  { id: 16, name: "Hoodie 2" },
  { id: 17, name: "Hoodie 3" },
  { id: 18, name: "Hoodie 4" }
])

const selectedUserItemsIds = ref<number[]>([])
const selectedChoiceItem = ref<Item | null>(null)

// Выбранные вещи пользователя в порядке выбора
const selectedUserItems = computed(() => {
  return selectedUserItemsIds.value
    .map(id => userItems.value.find(item => item.id === id))
    .filter((item): item is Item => item !== undefined)
})

const isUserItemSelected = (id: number): boolean => {
  return selectedUserItemsIds.value.includes(id)
}

// Переключение выбора вещи пользователя
const toggleUserItem = (item: Item) => {
  const index = selectedUserItemsIds.value.indexOf(item.id)
  
  if (index > -1) {
    selectedUserItemsIds.value.splice(index, 1)
  } else {
    if (selectedUserItemsIds.value.length < 6) {
      selectedUserItemsIds.value.push(item.id)
    }
  }
}

// Выбор вещи на выбор (только одна)
const selectChoiceItem = (item: Item) => {
  if (selectedChoiceItem.value?.id === item.id) {
    selectedChoiceItem.value = null
  } else {
    selectedChoiceItem.value = item
  }
}
</script>

<style scoped>
.app-container {
  max-width: 1000px;
  width: 100%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.top-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.bottom-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.clothes{
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.clothes,
.bottom-col{
  background: #2d2d2d;
  border: 2px solid #c77dff;
  border-radius: 8px;
  padding: 20px;
}

.clothes h3,
.bottom-col h3 {
  font-size: 16px;
  color: #e0e0e0;
}
.bottom-col h3{
  margin-bottom: 16px;
}

.selected-items {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.selected-item {
  background: #1a5490;
  border: 2px solid #c77dff;
  border-radius: 4px;
  padding: 10px 15px;
  font-size: 14px;
  color: #a8d5ff;
}

.selected-item-large {
  display: flex;
  align-items: center;
  justify-content: center;
  flex:1 1 auto;
}

.info-message {
  color: #888;
  font-style: italic;
}

.items-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(80px, 1fr));
  gap: 10px;
}

.item-card {
  background: #1e1e1e;
  border: 2px solid #a5a5a5;
  border-radius: 4px;
  padding: 15px 5px;
  text-align: center;
  cursor: pointer;
  transition: all 0.2s;
  font-size: 14px;
  color: #e0e0e0;
}

.item-card:hover {
  background: #333;
  border-color: #c77dff;
}

.item-card.selected {
  background: #1d343d;
  border-color: #6de0d1;
}


@media (max-width: 768px) {
  .top-section {
    grid-template-columns: 1fr;
  }
  .bottom-section,.top-section{
    gap: 10px;
  }
  .bottom-col,.clothes{
    padding: 10px;
  }
}

</style>
