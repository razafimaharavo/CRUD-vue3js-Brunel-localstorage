<template>
  <div id="app">
    <div class="container">
      <h1 class="mt-5 mb-3" style="color:white; text-align:center">Liste des données à traiter</h1> <br>
      <div class="data-container">
        <!-- Utilisation de DataCard et passage de la prop items -->
        <DataCard :items="items" @save="saveChanges" @delete="deleteItem" />
      </div>
    </div>
  </div>
</template>

<script>
import DataCard from "./components/DataCard.vue";

export default {
  name: "App",
  components: {
    DataCard
  },
  data() {
    return {
      items: []
    };
  },
  mounted() {
    const localItems = localStorage.getItem("items");
    if (localItems) {
      this.items = JSON.parse(localItems);
    } else {
      this.items = [
        { id: 1, name: "Donnée 1", email: "email1@example.com", devise: "USD" },
        { id: 2, name: "Donnée 2", email: "email2@example.com", devise: "EUR" },
        { id: 3, name: "Donnée 3", email: "email3@example.com", devise: "GBP" }
      ];
    }
  },
  // methods: {
  //   saveChanges(updatedItem) {
  //     const index = this.items.findIndex(item => item.id === updatedItem.id);
  //     if (index !== -1) {
  //       this.items.splice(index, 1, updatedItem);
  //       localStorage.setItem("items", JSON.stringify(this.items));
  //     }
  //   },
  //   deleteItem(itemId) {
  //     this.items = this.items.filter(item => item.id !== itemId);
  //     localStorage.setItem("items", JSON.stringify(this.items));
  //   }
  // }
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
}
.data-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
</style>
