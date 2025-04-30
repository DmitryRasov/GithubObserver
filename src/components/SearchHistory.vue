<template>
  <div class="history-sidebar" :class="{ 'history-sidebar--open': isOpen }">
    <button class="history-toggle" @click="toggleSidebar">
      <img 
        src="../../public/history.svg" 
        alt="history icon"
        class="history-icon"
      >
    </button>
    <div class="history-content">
      <h3>История поиска</h3>
      <div 
        v-for="(item, index) in history" 
        :key="index"
        class="history-item"
      >
        <div class="history-header">
          <div class="history-main-content" @click="toggleItem(index)">
            <div class="history-query">{{ item.query }}</div>
            <div class="history-emails-count">
              Найдено: {{ item.emails.length }}
            </div>
          </div>
          <button class="delete-button" @click.stop="deleteItem(index)">
            <img src="../../public/delete.svg" alt="delete" class="delete-icon">
          </button>
        </div>
        <div class="history-emails" v-if="expandedItems[index]">
          <div 
            v-for="(email, emailIndex) in item.emails" 
            :key="emailIndex"
            class="email-item"
            @click="selectHistoryItem(item)"
          >
            {{ email }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SearchHistory',
  props: {
    history: {
      type: Array,
      required: true,
      default: () => []
    }
  },
  data() {
    return {
      isOpen: false,
      expandedItems: {}
    }
  },
  methods: {
    toggleSidebar() {
      this.isOpen = !this.isOpen
    },
    toggleItem(index) {
      this.expandedItems[index] = !this.expandedItems[index]
    },
    selectHistoryItem(item) {
      this.$emit('select-history', item)
    },
    deleteItem(index) {
      this.$emit('delete-history', index)
    }
  }
}
</script>

<style scoped>
.history-sidebar {
  position: fixed;
  right: -300px;
  top: 0;
  height: 100vh;
  width: 300px;
  background: #2e2e2e;
  transition: right 0.3s ease;
  z-index: 1000;
  padding: 20px;
  box-shadow: -2px 0 5px rgba(0, 0, 0, 0.2);
}

.history-sidebar--open {
  right: 0;
}

.history-toggle {
  position: absolute;
  left: -40px;
  top: 20px;
  background: #2e2e2e;
  border: none;
  border-radius: 5px 0 0 5px;
  padding: 10px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.history-toggle:hover {
  background: #3e3e3e;
}

.history-icon {
  width: 20px;
  height: 20px;
  filter: invert(92%) sepia(45%) saturate(2672%) hue-rotate(355deg) brightness(105%) contrast(102%);
}

.history-content {
  color: #ffe500;
  height: 100%;
  overflow-y: auto;
}

.history-content h3 {
  margin-bottom: 20px;
  text-align: center;
}

.history-item {
  background: #3e3e3e;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.history-item:hover {
  background: #4e4e4e;
}

.history-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 5px;
}

.history-main-content {
  flex: 1;
  cursor: pointer;
}

.history-query {
  font-weight: bold;
  margin-bottom: 5px;
}

.history-emails-count {
  font-size: 0.9em;
  opacity: 0.8;
}

.history-emails {
  margin-top: 10px;
  padding-top: 10px;
  border-top: 1px solid #4e4e4e;
}

.email-item {
  padding: 5px;
  font-size: 0.9em;
  word-break: break-all;
  cursor: pointer;
  transition: background 0.3s ease;
}

.email-item:hover {
  background: #4e4e4e;
}

.delete-button {
  background: none;
  border: none;
  padding: 5px;
  cursor: pointer;
  opacity: 0.7;
  transition: opacity 0.3s ease;
}

.delete-button:hover {
  opacity: 1;
}

.delete-icon {
  width: 16px;
  height: 16px;
  filter: invert(92%) sepia(45%) saturate(2672%) hue-rotate(355deg) brightness(105%) contrast(102%);
}
</style> 