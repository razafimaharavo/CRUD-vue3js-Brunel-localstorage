<template>
  <button onclick="document.getElementById('AjoutDonneeModal').style.display='block'"
    style="position: fixed; top: 36pc; left: 78pc;" class="w3-button w3-circle w3-teal w3-xlarge">+</button>

  <div class="container"></div>
  <div class="row justify-content-center">
    <div class="col-md-12" style="background-color: transparent;">
      <div class="w3-card-4">

        <h1 class="Sorabe">Liste des données à traiter</h1>

        <div class="form_div">
          <input class="input_searchko" type="text" v-model="searchQuery" placeholder="">
          <label for="newName" class="form_label">Rechercher ...</label>
        </div>

        <table class="table_with">
          <thead>
            <tr>
              <th>ID</th>
              <th>Nom</th>
              <th>Email</th>
              <th>Devise</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>
            <!-- Utilisez les variables réactives et les fonctions définies -->
            <tr v-for="item in filteredItems" :key="item.id">
              <td>{{ item.id }}</td>
              <td>
                <input class="input_table" type="text" v-model="item.name" @focus="startEditing(item)">
              </td>
              <td>
                <input class="input_table" type="text" v-model="item.email" @focus="startEditing(item)">
              </td>
              <td>
                <input class="input_table" type="text" v-model="item.devise" @focus="startEditing(item)">
              </td>
              <td>
                <button @click="saveChanges(item)" :disabled="item.id !== editingId"
                  class="btnEnregistre w3-round-large">Enregistrer</button>
                <button @click="confirmDelete(item)" class="btn btn-danger w3-round-large">Supprimer</button>
              </td>
            </tr>
          </tbody>
        </table>


        <div id="AjoutDonneeModal" class="w3-modal">
          <div class="w3-modal-content w3-animate-zoom w3-round-xlarge" style="width: 500px">
            <div class="w3-container w3-black w3-display-container"
              style="background: linear-gradient(to bottom right, #000000 0%, #66ccff 100%); text-align:center">
              <button onclick="document.getElementById('AjoutDonneeModal').style.display='none'"
                class="w3-button w3-display-topright w3-large">x</button>
              <div class="card-header">
                <h2>Ajouter de nouvelles données</h2>
              </div>
              <hr>
            </div>
            <div>
              <div class="card-body w3-padding-xlarge">
                <div class="input_group">
                  <!-- <h2>Ajouter de nouvelles données</h2> -->
                  <div class="form_div">
                    <input id="newName" class="form_input" type="text" v-model="newItem.name" placeholder=" " required>
                    <label for="newName" class="form_label">Nom ..</label>
                  </div>
                  <div class="form_div">
                    <input id="newEmail" class="form_input" type="email" v-model="newItem.email" placeholder=" "
                      required>
                    <label for="newEmail" class="form_label">Email ..</label>
                  </div>
                  <div class="form_div">
                    <input id="newDevise" class="form_input" type="text" v-model="newItem.devise" placeholder=" "
                      required>
                    <label for="newDevise" class="form_label">Devise ..</label>
                  </div>
                  <button @click="addItem" class="btn btn-outline-info">Ajouter</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script setup>
// import { ref, computed, onMounted } from 'vue';
import { ref, computed, onMounted, onBeforeUnmount } from 'vue';

const items = ref([]);
const newItem = ref({ name: '', email: '', devise: '' });
const searchQuery = ref('');
const editingId = ref(null);

// Chargez les données depuis le localStorage lors du montage du composant et les met dans mon vairable items (c'est un tableau)
onMounted(() => {
  if (localStorage.getItem('items')) {
    items.value = JSON.parse(localStorage.getItem('items'));
  }
});

//Zavatra ilaina mba hi-enregistrena anles données ao @ localstorage
// onMounted(async () => {
//   try {
//     const response = await fetch('/data.json');
//     const data = await response.json();
//     items.value = data;
//   } catch (error) {
//     console.error('Erreur lors du chargement des données :', error);
//   }
// });

// Fonction pour charger les données depuis localStorage lors du montage du composant
//  const loadItemsFromLocalStorage = () => {
//     const storedData = localStorage.getItem('items');
//     if (storedData) {
//       items.value = JSON.parse(storedData);
//     }
//   };

//   // Charger les données depuis localStorage lors du montage du composant
//   onMounted(loadItemsFromLocalStorage);

//   // Sauvegarder les données dans localStorage avant le démontage du composant
//   onBeforeUnmount(() => {
//     localStorage.setItem('items', JSON.stringify(items.value));
//   });

// Filtrer les éléments en fonction de la recherche
const filteredItems = computed(() => {
  return items.value.filter(item => {
    return (
      item.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      item.email.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      item.devise.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  });
});

// Démarrez la modification de l'élément
const startEditing = (item) => {
  editingId.value = item.id;
};

// Enregistrez les modifications de l'élément
const saveChanges = (item) => {
  editingId.value = null;
  localStorage.setItem('items', JSON.stringify(items.value));
  alert("La modification est réussie avec succès");
};

// Demandez confirmation avant de supprimer l'élément
const confirmDelete = (item) => {
  if (confirm("Voulez-vous vraiment supprimer l'élément \"" + item.name + "\" ?")) {
    deleteItem(item.id);
  }
};

// Supprimez un élément
const deleteItem = (id) => {
  items.value = items.value.filter(item => item.id !== id);
  localStorage.setItem('items', JSON.stringify(items.value));
};

// Ajoutez un nouvel élément
const addItem = () => {
  if (newItem.value.name && newItem.value.email && newItem.value.devise) {
    const newId = items.value.length > 0 ? items.value[items.value.length - 1].id + 1 : 1;
    items.value.push({
      id: newId,
      name: newItem.value.name,
      email: newItem.value.email,
      devise: newItem.value.devise
    });
    newItem.value = { name: '', email: '', devise: '' };
    localStorage.setItem('items', JSON.stringify(items.value));
    alert("L'insertion est réussie avec succès");
  } else {
    alert("Veuillez remplir tous les champs avant d'ajouter une nouvelle donnée.");
  }
};
</script>



<style scoped>
@import '/css/bootstrap.min.css';
@import '/css/w3.css';

body {
  font-family: Arial, sans-serif;
}

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

.table_with{
  width: 1000px;
  position: relative;
  left: 40px;
}

.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  width: 250pc;
  background-image: url(BackgoundImageKo/fondNaka.png);
  opacity: 0.3;
  background-position: center;
  background-size: cover;
  position: fixed;
  top: 0pc;
  z-index: -2;
  transition: 0.5s ease;
  transform-style: preserve-3d;
}

.add-form {
  max-width: 400px;
  margin-top: 20px;
}

.Sorabe {
  color: teal;
  text-align: center;
}

.input_table {
  border: none;
  background-color: transparent;
  height: 35px;
  color:rgb(209, 208, 208);
}

.input_group {
  /* top: 10px;
    position: relative;
    left: 20px; */
  transition: 0.5s;
}

.form_div {
  width: 100%;
  margin-top: 10px;
  position: relative;
  height: 48px;
  margin-bottom: 1.5rem;
}

.input_searchko {
  font-size: 1rem;
  width: 100%;
  height: 100%;
  border: none;
  border-radius: .5rem;
  outline: none;
  padding: 1rem;
  background: transparent;
  z-index: 1;
  transition: 0.3s;
}

.input_searchko:focus+.form_label {
  top: -0.5rem;
  left: 0.8rem;
  color: #fff;
  font-size: 0.75rem;
  font-weight: 500;
  z-index: 10;
  background-color: #194f68;
  border-radius: 10px;
  padding: 0 5px;
}

.input_searchko:not(:placeholder-shown).form_input:not(:focus)+.form_label {
  top: -0.5rem;
  left: 0.8rem;
  font-size: 0.75rem;
  font-weight: 500;
  background-color: #194f68;
  border-radius: 10px;
  color: #fff;
  z-index: 10;
}

.input_searchko:focus {
  border: 1.5px solid #194f68;
}


.form_input {
  font-size: 1rem;
  width: 100%;
  height: 100%;
  border: none;
  box-shadow: 0 0 20px 9px #1b1f2955;
  border-radius: .5rem;
  outline: none;
  padding: 1rem;
  background: transparent;
  z-index: 1;
  transition: 0.3s;
}

.form_label {
  position: absolute;
  left: 1rem;
  top: 1rem;
  padding: 0 0.25rem;
  background-color: transparent;
  color: #000;
  font-size: 1rem;
  transition: 0.3s;
}

.form_input:focus+.form_label {
  top: -0.5rem;
  left: 0.8rem;
  color: #fff;
  font-size: 0.75rem;
  font-weight: 500;
  z-index: 10;
  background-color: #35b9f6;
  border-radius: 10px;
  padding: 0 5px;
}

.form_input:not(:placeholder-shown).form_input:not(:focus)+.form_label {
  top: -0.5rem;
  left: 0.8rem;
  font-size: 0.75rem;
  font-weight: 500;
  background-color: #35b9f6;
  border-radius: 10px;
  color: #fff;
  z-index: 10;
}

.form_input:focus {
  border: 1.5px solid #35b9f6;
}
</style>
