<template>
  <div class="w3-card-4 card-container">
    <h4> <strong>{{ item.name }}</strong> </h4>
    <p> {{ item.email }}</p>
    <p>Devise: {{ item.devise }}</p>
    <div>
      <button @click="editItem" class="btn btn-warning w3-round-xlarge">Modifier</button>
      <button @click="confirmDelete" class="btn btn-danger w3-round-xlarge">Supprimer</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  methods: {
    editItem() {
      this.$emit("edit", this.item);
    },
    confirmDelete() {
      if (confirm("Voulez-vous vraiment supprimer cet élément ?")) {
        this.$emit("delete", this.item);
      }
    }
  }
};
</script>

<style scoped>
@import '/css/bootstrap.min.css';
@import '/css/w3.css';
/* Style spécifique à la carte */
/* .card-container {
  width: 30%; 
  padding: 20px;
  margin-bottom: 20px;
} */

/* Style pour la carte de W3.CSS */
/* .w3-card-4 {
  background-color: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
} */

/* Style pour les boutons */
button {
  margin-top: 10px;
  padding: 10px 20px;
  cursor: pointer;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
}

button:hover {
  background-color: #0056b3;
}
</style>
