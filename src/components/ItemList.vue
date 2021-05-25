<template>
  <div>
    <div>ItemList</div>
    <div v-for="item in items" :key="item.name">
      <div class="item">
        <div class="name">名前: {{ item.name }}</div>
        <div class="price">{{ item.price }} 円</div>
      </div>
    </div>
    <div>
      <label>
        名前
        <input v-model="newItemName" type="text" />
      </label>
      <label>
        価格
        <input v-model="newItemPrice" type="number" />
      </label>
      <button @click="addItem">add</button>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const items = ref([
      { name: "たまご", price: 100 },
      { name: "りんご", price: 160 },
    ]);
    const newItemName = ref("");
    const newItemPrice = ref(0);

    const resetNewItem = () => {
      newItemName.value = "";
      newItemPrice.value = 0;
    };

    const addItem = () => {
      if (newItemName.value == "") {
        alert("名前を入力してください");
        return;
      }

      items.value.push({ name: newItemName.value, price: newItemPrice.value });
      resetNewItem();
    };

    return { items, newItemName, newItemPrice, addItem };
  },
};
</script>

<style></style>
