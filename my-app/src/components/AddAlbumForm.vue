<script setup lang="ts">
import { defineProps, defineEmits, ref, watch } from "vue";
import type { Album } from "../types";

const props = defineProps<{ prefilledAlbum?: Album | null }>();
const emit = defineEmits<{ (event: "update:albums", album: Album): void }>();

const album = ref<Album>({
  id: 0,
  name: "",
  description: "",
  stock: 0,
  price: 0,
});

watch(
  () => props.prefilledAlbum,
  (newVal) => {
    if (newVal) {
      album.value = { ...newVal, id: 0 };
    }
  },
  { immediate: true }
);

const addAlbum = () => {
  emit("update:albums", { ...album.value });
  album.value = { id: 0, name: "", description: "", stock: 0, price: 0 };
};
</script>

<template>
  <div class="card p-3">
    <h3>{{ prefilledAlbum ? "Album Dupliqué" : "Ajouter un Nouvel Album" }}</h3>
    <div class="mb-2">
      <label>Nom</label>
      <input v-model="album.name" class="form-control" />
    </div>
    <div class="mb-2">
      <label>Description</label>
      <input v-model="album.description" class="form-control" />
    </div>
    <div class="mb-2">
      <label>Quantité</label>
      <input type="number" v-model="album.stock" class="form-control" />
    </div>
    <div class="mb-2">
      <label>Prix</label>
      <input type="number" v-model="album.price" class="form-control" />
    </div>
    <button class="btn btn-primary" @click="addAlbum">Enregistrer</button>
  </div>
</template>

<style scoped></style>
