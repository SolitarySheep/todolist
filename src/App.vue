<template>
  <div>
    <nav class="navbar bg-body-tertiary">
      <div class="container-fluid">
        <a class="navbar-brand">Navbar</a>
        <form class="d-flex" role="search">
          <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
      </div>
    </nav>

    <div class="container mt-3">
      <h2>Ajoutez des tâches</h2>
      <form @submit.prevent="ajouterTache">
        <div class="mb-3">
          <label for="tache" class="form-label">Tâche</label>
          <input v-model="nomTache" type="text" class="form-control" id="tache" placeholder="Un truc à faire">
        </div>
        <button type="submit" class="btn btn-primary">Ajoutez</button>
      </form>

      <h2>Liste des tâches</h2>
      <ul class="list-group mt-3">
        <li v-for="(tache, index) in taches" :key="index" class="list-group-item">
          <div class="d-flex justify-content-between align-items-center">
            <div class="form-check">
              <input v-model="tache.estFait" class="form-check-input" type="checkbox" :id="'check-' + index">
              <label class="form-check-label" :for="'check-' + index">{{ tache.nom }}</label>
            </div>
            <button @click="demanderSuppression(index)" class="btn btn-danger">Supprimer</button>
          </div>
        </li>
      </ul>
    </div>
  </div>

  <ConfirmDialog
  v-if="confirmation.visible"
  :message="confirmation.message"
  @onConfirm="confirmerSuppression(suppressionEnCours.value)"
  @onCancel="annulerSuppression"
/>


</template>

<script setup>
import { ref } from 'vue';
import ConfirmDialog from './ConfirmDialog.vue'; // Assurez-vous d'ajuster le chemin du composant selon votre structure de fichiers

const taches = ref([]);
const nomTache = ref("");
const estInvalide = ref(false);

const ajouterTache = () => {
  if (nomTache.value === "") {
    estInvalide.value = true;
    return;
  }

  taches.value.push({ nom: nomTache.value, estFait: false });
  nomTache.value = "";
};

const suppressionEnCours = ref(null);

const demanderSuppression = (index) => {
  confirmation.value.message = 'Êtes-vous sûr de vouloir supprimer cette tâche ?';
  confirmation.value.visible = true;
};


const confirmerSuppression = (index) => {
  if (index !== null) {
    taches.value.splice(index, 1);
    confirmation.value.visible = false;
  }
};

const annulerSuppression = () => {
  suppressionEnCours.value = null;
  confirmation.value.visible = false;
};

const confirmation = ref({ visible: false, message: '' });
</script>

