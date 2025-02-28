<script setup lang="ts">
import { ref, defineEmits } from "vue";
import type { Album } from "../types";

const emit = defineEmits(["update:albums"]);

const nextId = ref<number>(1);
const newAlbumName = ref<string>("");
const newAlbumDescription = ref<string>("");
const newAlbumStock = ref<number>(0);
const newAlbumPrice = ref<number>(0);
const albums = ref<Album[]>([]);

const submitAlbum = () => {
  const newAlbum: Album = {
    id: nextId.value++,
    name: newAlbumName.value,
    description: newAlbumDescription.value,
    stock: newAlbumStock.value,
    price: newAlbumPrice.value,
  };

  albums.value.push(newAlbum);
  emit("update:albums", albums.value);

  newAlbumName.value = "";
  newAlbumDescription.value = "";
  newAlbumStock.value = 0;
  newAlbumPrice.value = 0;
};
</script>

<template>
  <form @submit.prevent="submitAlbum" class="container mt-4">
    <div class="mb-3">
      <label for="name" class="form-label">Nom:</label>
      <input v-model="newAlbumName" id="name" class="form-control" required />
    </div>
    <div class="mb-3">
      <label for="description" class="form-label">Description:</label>
      <input
        v-model="newAlbumDescription"
        id="description"
        class="form-control"
        required
      />
    </div>
    <div class="mb-3">
      <label for="stock" class="form-label">Quantité:</label>
      <input
        v-model.number="newAlbumStock"
        id="stock"
        type="number"
        class="form-control"
        required
      />
    </div>
    <div class="mb-3">
      <label for="price" class="form-label">Prix:</label>
      <input
        v-model.number="newAlbumPrice"
        id="price"
        type="number"
        step="0.01"
        class="form-control"
        required
      />
    </div>
    <button type="submit" class="btn btn-primary">Créer un Album</button>
  </form>
</template>

<style scoped></style>
