<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import { type Album } from "../types";

const props = defineProps<{ album: Album }>();

const emit = defineEmits<{
  (event: "deleteAlbum", albumId: number): void;
}>();
</script>

<template>
  <li class="list-group-item d-flex align-items-center justify-content-between">
    <div class="d-flex flex-column">
      <h3 class="m-0">{{ album.name }}</h3>
      <p class="mb-1"><strong>Description:</strong> {{ album.description }}</p>
      <p class="mb-1"><strong>Quantité:</strong> {{ album.stock }}</p>
      <p class="mb-1"><strong>Prix:</strong> {{ album.price }}</p>
    </div>
    <button
      class="btn btn-danger btn-sm"
      @click="emit('deleteAlbum', album.id)"
    >
      <i class="bi bi-trash"></i> Supprimer
    </button>
  </li>
</template>

<style scoped></style>
