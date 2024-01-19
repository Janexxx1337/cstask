<template>
    <div class="containers">
      <div class="container">
        <VirtualScroller :key="itemsKey" :items="items" :itemSize="70" class="scroller">
          <template #item="{ item }">
            <div class="item">
              <img :src="item.image" alt="" loading="lazy" />
              <div>Цена: {{ item.price.toFixed(2) }}</div>
              <div>Уровень: {{ item.level }}</div>
              <button @click="moveToContainer2(item)">Переместить</button>
            </div>
          </template>
        </VirtualScroller>
      </div>
  
      <div class="container">
        <VirtualScroller :key="items2Key" :items="items2" :itemSize="70" class="scroller">
          <template #item="{ item }">
            <div class="item">
              <img :src="item.image" alt="" loading="lazy" />
              <div>Цена: {{ item.price.toFixed(2) }}</div>
              <div>Уровень: {{ item.level }}</div>
              <button @click="moveToContainer1(item)">Вернуть</button>
            </div>
          </template>
          <template #empty>
            <div class="empty">Контейнер пуст</div>
          </template>
        </VirtualScroller>
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue';
  import VirtualScroller from 'primevue/virtualscroller';
  
  interface Item {
    id: number;
    price: number;
    level: number;
    image: string;
  }
  const itemsKey = ref(0);
  const items2Key = ref(0);
  const items3Key = ref(3);
  
  const updateKeys = () => {
    itemsKey.value++;
    items2Key.value++;
  };
  
  const createItem = (id: number): Item => ({
    id,
    price: Math.random() * 100,
    level: Math.floor(Math.random() * 10),
    image: `https://placekitten.com/${50 + id % 10}/${50 + id % 10}`,
  });
  
  const items = ref<Item[]>(Array.from({ length: 10000 }, (_, i) => createItem(i)));
  const items2 = ref<Item[]>([]);
  
  const moveToContainer2 = (item: Item) => {
    const index = items.value.findIndex(i => i.id === item.id);
    if (index !== -1) {
      items.value.splice(index, 1);
      items2.value.push(item);
      updateKeys();
    }
  };
  
  const moveToContainer1 = (item: Item) => {
    const index = items2.value.findIndex(i => i.id === item.id);
    if (index !== -1) {
      items2.value.splice(index, 1);
      items.value.push(item);
      updateKeys();
    }
  };
  
  
  </script>
  
  <style scoped>
  .containers {
    display: flex;
    justify-content: space-between;
  }
  
  .containers {
    display: flex;
    justify-content: space-between;
  }
  
  .container {
    width: 50%;
    height: 500px;
    overflow: auto;
  }
  
  .scroller {
    height: 100%;
  }
  
  .item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 70px;
  }
  
  .empty {
    text-align: center;
    padding: 20px;
  }
  
  button {
    margin-left: 10px;
  }
  </style>
  