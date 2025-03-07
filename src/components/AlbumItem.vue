<script setup lang="ts">
import { defineProps, defineEmits, ref } from "vue";
import type { Album } from "../types";

defineProps<{ album: Album }>();

const emit = defineEmits<{
  (event: "editAlbum", album: Album): void;
  (event: "duplicateAlbum", album: Album): void;
  (event: "deleteAlbum", albumId: number): void;
}>();

const showDetails = ref(false);

const toggleDetails = () => {
  showDetails.value = !showDetails.value;
};

// Method to apply the correct text color for the stock value
const stockColor = (stock: number) => {
  if (stock === 0) return "text-danger"; // Red for 0 stock
  if (stock <= 5) return "text-warning"; // Orange for 5 or less
  return "text-success"; // Green for more than 5
};
</script>

<template>
  <li class="list-group-item d-flex align-items-center justify-content-between">
    <div class="d-flex align-items-center gap-4 w-100">
      <h3 class="m-0">{{ album.name }}</h3>

      <div v-if="showDetails" class="d-flex gap-4">
        <p class="m-0"><strong>Description:</strong> {{ album.description }}</p>

        <!-- Apply color to both Quantité and the stock value -->
        <p class="m-0">
          <strong class="d-inline" :class="stockColor(album.stock)"
            >Quantité:</strong
          >
          <span :class="stockColor(album.stock)">{{ album.stock }}</span>
        </p>

        <p class="m-0"><strong>Prix:</strong> {{ album.price }}$</p>
      </div>
    </div>

    <div class="d-flex gap-2">
      <button class="btn btn-info btn-sm" @click="toggleDetails">
        {{ showDetails ? "Voir Moins" : "Voir Plus" }}
      </button>

      <button class="btn btn-warning btn-sm" @click="emit('editAlbum', album)">
        <i class="bi bi-pencil"></i> Modifier
      </button>
      <button
        class="btn btn-success btn-sm"
        @click="emit('duplicateAlbum', album)"
      >
        <i class="bi bi-files"></i> Dupliquer
      </button>
      <button
        class="btn btn-danger btn-sm"
        @click="emit('deleteAlbum', album.id)"
      >
        <i class="bi bi-trash"></i> Supprimer
      </button>
    </div>
  </li>
</template>

<style scoped></style>
