<template>
  <section class="shopping-list-container">
    <header class="header">
      <h1>{{ header }}</h1>

      <button
        class="btn"
        :class="!isEditingEnabled ? 'btn-primary' : 'null'"
        @click="setEditingModeOn"
        :title="!isEditingEnabled ? 'Add Item' : 'Cancel'"
      >
        {{ !isEditingEnabled ? "Add Item" : "Cancel" }}
      </button>
    </header>

    <form
      class="add-item-form"
      @submit.prevent="saveItem"
      v-if="isEditingEnabled"
    >
      <label class="sr-only" for="inputValue"> Input item: </label>
      <input
        id="inputValue"
        type="text"
        placeholder="Item with space..."
        v-model.trim="trimmedInputItem"
      />

      <label for="priorityCheckbox"
        ><input
          id="priorityCheckbox"
          type="checkbox"
          v-model="hasHighPriority"
        />
        High Priority</label
      >

      <button
        class="btn btn-primary"
        :disabled="trimmedInputItem.length === 0"
        :title="checkInputEmpty ? 'Input field is empty' : 'Save Item'"
      >
        Save Item
      </button>
    </form>

    <p class="counter">{{ countCharacterLength }}/200</p>

    <ul>
      <li
        v-for="item in reversedItems"
        :key="item.id"
        :class="{ strikeout: item.purchased, priority: item.highPriority }"
        @click="togglePurchasedItem(item)"
      >
        {{ item.product }}
      </li>
    </ul>
    <p v-if="!items.length">Shopping list is empty!</p>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";
const header = ref("Shopping List App");
const items = ref([
  {
    id: 1,
    product: "Milk",
    purchased: true,
    highPriority: false,
  },
  {
    id: 2,
    product: "Yoghurt",
    purchased: false,
    highPriority: true,
  },
  {
    id: 3,
    product: "Banana",
    purchased: true,
    highPriority: false,
  },
  {
    id: 4,
    product: "Apples",
    purchased: false,
    highPriority: true,
  },
  {
    id: 5,
    product: "Juice",
    purchased: true,
    highPriority: true,
  },
]);

const trimmedInputItem = ref("");
const hasHighPriority = ref(false);

const isEditingEnabled = ref(false);

const saveItem = () => {
  items.value.push({
    id: items.value.length + 1,
    product: trimmedInputItem.value,
    highPriority: hasHighPriority.value,
  });
  trimmedInputItem.value = "";
  hasHighPriority.value = "";
};

const setEditingModeOn = () => {
  isEditingEnabled.value = !isEditingEnabled.value;
  trimmedInputItem.value = "";
  hasHighPriority.value = "";
};

const togglePurchasedItem = (item) => {
  item.purchased = !item.purchased;
};

const countCharacterLength = computed(() => {
  return trimmedInputItem.value.length;
});

const reversedItems = computed(() => {
  return [...items.value].reverse();
});

const checkInputEmpty = () => trimmedInputItem.value.length === 0;
</script>

<style scoped>
.sr-only:not(:focus):not(:active) {
  clip: rect(0 0 0 0);
  clip-path: inset(50%);
  height: 1px;
  overflow: hidden;
  position: absolute;
  white-space: nowrap;
  width: 1px;
}

.shopping-list-container {
  background: lightyellow;
  padding: 2rem;
  margin: 1rem;
  border-radius: 3px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.12), 0 2px 4px 0 rgba(0, 0, 0, 0.08);
}

h1 {
  color: teal;
  font-size: 2rem;
}

ul {
  list-style: none;
  padding: 0;
}

a {
  color: #6cb2eb;
  font-size: 1.25rem;
  transition: all 0.1s ease-in;
  margin-top: 0.5rem;
}

a:hover {
  color: #3490dc;
}

li,
p {
  display: flex;
  align-items: center;
  line-height: 1.75;
  letter-spacing: 0.5px;
  color: #3d4852;
  font-size: 1.25rem;
  cursor: pointer;
  transition: all 0.1s ease-in;
}

li:hover {
  color: #22292f;
}

li input {
  margin: 0 0.5rem 0;
}

.counter {
  font-size: 0.8rem;
  padding-left: 10px;
  padding-right: 10px;
}

#shopping-list > input,
#shopping-list > select {
  width: 100%;
  border-radius: 3px;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);
  border: 1px solid #f1f5f8;
  color: #606f7b;
  padding: 0.5rem 0.75rem;
  box-sizing: border-box;
  font-size: 1rem;
  letter-spacing: 0.5px;
  margin: 0.5rem 0;
}

.add-item-form,
.header {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.add-item-form input {
  width: 100%;
  border-radius: 3px;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);
  border: 1px solid #f1f5f8;
  color: #606f7b;
  padding: 0.5rem 0.75rem;
  box-sizing: border-box;
  font-size: 1rem;
  letter-spacing: 0.5px;
  margin: 0.5rem 0;
}

.btn {
  border: none;
  border-radius: 3px;
  margin: auto 0;
  padding: 0.5rem 0.75rem;
  flex-shrink: 0;
  cursor: pointer;
  font-size: 0.9rem;
  letter-spacing: 0.5px;
  transition: all 0.1s ease-in;
}

.btn[disabled] {
  background: #8795a1;
}

.btn[disabled]:hover {
  background: #606f7b;
  cursor: not-allowed;
}

.btn-primary {
  background: #6cb2eb;
  color: #fff;
}

.btn-primary:hover {
  background: #3490dc;
}

.btn-cancel {
  background: #ef5753;
  color: #fff;
}

.btn-cancel:hover {
  background: #e3342f;
  color: #fff;
}

.strikeout {
  text-decoration: line-through;
  color: #b8c2cc;
}

.strikeout:hover {
  color: #8795a1;
}

.priority {
  color: #de751f;
}

input[type="checkbox"] {
  display: inline !important;
  box-shadow: none;
  width: auto;
}

.show-new-item {
  color: red;
}

.show-trim-item {
  color: green;
}
</style>