<script setup lang="ts">
import { ref, defineProps, defineEmits, watch } from "vue";
import type { Album } from "../types";

const props = defineProps<{ album: Album }>();
const emit = defineEmits(["update:album"]);

const updatedAlbumName = ref(props.album.name);
const updatedAlbumDescription = ref(props.album.description);
const updatedAlbumStock = ref(props.album.stock);
const updatedAlbumPrice = ref(props.album.price);

watch(
  () => props.album,
  (newAlbum) => {
    updatedAlbumName.value = newAlbum.name;
    updatedAlbumDescription.value = newAlbum.description;
    updatedAlbumStock.value = newAlbum.stock;
    updatedAlbumPrice.value = newAlbum.price;
  },
  { deep: true }
);

const submitUpdate = () => {
  const updatedAlbum: Album = {
    ...props.album,
    name: updatedAlbumName.value,
    description: updatedAlbumDescription.value,
    stock: updatedAlbumStock.value,
    price: updatedAlbumPrice.value,
  };

  emit("update:album", updatedAlbum);
};

const cancelEdit = () => {
  // This can be adjusted to whatever you want to happen on cancel (e.g., go back to previous state)
  emit("update:album", props.album); // Reset to original album
};
</script>

<template>
  <div class="card p-4 mb-4 shadow-sm">
    <h3 class="card-title mb-3">Modifier l'Album</h3>
    <form @submit.prevent="submitUpdate">
      <div class="mb-3">
        <label for="name" class="form-label">Nom:</label>
        <input
          v-model="updatedAlbumName"
          id="name"
          class="form-control"
          required
        />
      </div>
      <div class="mb-3">
        <label for="description" class="form-label">Description:</label>
        <input
          v-model="updatedAlbumDescription"
          id="description"
          class="form-control"
          required
        />
      </div>
      <div class="mb-3">
        <label for="stock" class="form-label">Quantité:</label>
        <input
          v-model.number="updatedAlbumStock"
          id="stock"
          type="number"
          class="form-control"
          required
        />
      </div>
      <div class="mb-3">
        <label for="price" class="form-label">Prix:</label>
        <input
          v-model.number="updatedAlbumPrice"
          id="price"
          type="number"
          step="0.01"
          class="form-control"
          required
        />
      </div>

      <!-- Submit and Cancel Buttons -->
      <div class="d-flex justify-content-between">
        <button type="submit" class="btn btn-primary">Modifier l'Album</button>
        <button type="button" class="btn btn-danger" @click="cancelEdit">
          Annuler
        </button>
      </div>
    </form>
  </div>
</template>
