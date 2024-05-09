<template>
  <div>
    <!-- Barre de recherche -->
    <input v-model="searchQuery" type="text" style="color:white" placeholder="Rechercher..." class="form-control mb-3">

    <div class="card-containera">
     <!-- Boucle sur les éléments filtrés et affiche chaque élément dans un composant ItemCard -->
      <ItemCard v-for="item in filteredItems" :key="item.id" :item="item" @edit="editItem" @delete="deleteItem" />
    </div>
    <!-- Bouton pour ajouter un nouvel élément -->
    <button @click="openModal('add')" class="w3-button w3-circle w3-teal AddBtn"> <strong class="PlusClass">+</strong></button>

    <!-- Modal pour l'édition/ajout -->
    <div v-if="isEditing" class="modal w3-modal-content w3-animate-zoom">
      <div class="modal-content w3-round-xlarge">
        <span class="close" @click="closeModal">&times;</span>
        <h2 style="text-align: center">{{ editingItem ? 'Modifier' : 'Ajouter' }} l'élément</h2>
        <label>Nom:</label>
        <input v-model="editedItem.name" type="text">
        <label>Email:</label>
        <input v-model="editedItem.email" type="email">
        <label>Devise:</label>
        <input v-model="editedItem.devise" type="text">
        <button @click="saveChanges">{{ editingItem ? 'Modifier' : 'Ajouter' }}</button>
      </div>
    </div>
  </div>
</template>

<script>
import ItemCard from "@/components/ItemCard .vue";

export default {
  components: { ItemCard },
  props: {
    items: { type: Array, required: true }
  },
  data() {
    return {
      isEditing: false,
      editingItem: null,
      editedItem: {
        id: '',
        name: '',
        email: '',
        devise: ''
      },
      searchQuery: ''
    };
  },
  computed: {
    filteredItems() {
      // Filtrer les éléments en fonction de la recherche
      const query = this.searchQuery.toLowerCase().trim();
      return this.items.filter(item => {
        return (
          item.name.toLowerCase().includes(query) ||
          item.email.toLowerCase().includes(query) ||
          item.devise.toLowerCase().includes(query)
        );
      });
    }
  },
  methods: {
    editItem(item) {
      this.isEditing = true;
      this.editingItem = true;
      // Cloner l'élément pour éviter de modifier l'original directement
      this.editedItem = { ...item };
    },
    openModal(mode) {
      this.isEditing = true;
      if (mode === 'add') {
        this.editingItem = false;
        // Réinitialiser les champs du formulaire pour l'ajout
        this.editedItem = {
          id: '', // Vous pouvez générer un nouvel ID ou utiliser une autre logique
          name: '',
          email: '',
          devise: ''
        };
      }
    },
    closeModal() {
      this.isEditing = false;
    },
    saveChanges() {
      if (this.editingItem) {
        // Logique pour sauvegarder les modifications
        const index = this.items.findIndex(item => item.id === this.editedItem.id);
        if (index !== -1) {
          // Mettre à jour les données de l'élément existant
          this.items[index] = { ...this.editedItem };
          // Mettre à jour les données dans le localStorage
          localStorage.setItem("items", JSON.stringify(this.items));
          this.closeModal();
        }
      } else {
        // Logique pour ajouter un nouvel élément
        const newItem = { ...this.editedItem };
        newItem.id = this.items.length > 0 ? this.items[this.items.length - 1].id + 1 : 1; // Génération d'un nouvel ID
        this.items.push(newItem);
        // Mettre à jour les données dans le localStorage
        localStorage.setItem("items", JSON.stringify(this.items));
        this.closeModal();
      }
    },
    deleteItem(item) {
      // Logique de suppression
      const index = this.items.findIndex(i => i.id === item.id);
      if (index !== -1) {
        this.items.splice(index, 1);
        // Mettre à jour les données dans le localStorage
        localStorage.setItem("items", JSON.stringify(this.items));
      } else {
        console.error("L'élément à supprimer n'a pas été trouvé !");
      }
    }
  }
};
</script>


<style scoped>
@import "/css/bootstrap.min.css";
@import "/css/w3.css";
/* Style spécifique à la carte */
.card-container {
  width: 85%; 
  padding: 20px;
  margin-bottom: 20px;
}

.card-containera {
  display: flex;
  flex-wrap: wrap; /* Permettre le passage à la ligne */
  justify-content: space-between; /* Espacement égal entre les éléments */
  padding: 20px;
  margin-bottom: 20px;
}

/* Style pour la carte de W3.CSS */
.w3-card-4 {
  background-color:#fff;
  border-radius: 10px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  width: 240px; /* Largeur de la carte */
  transition: 0.3s;
}

/* Style pour le modal */
.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 10% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 50%;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

.AddBtn{
  position: fixed;
  top: 35pc;
  left:72pc;
  width:65px;
}
.PlusClass{
  font-size: 30px;
}

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
