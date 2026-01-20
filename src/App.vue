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

