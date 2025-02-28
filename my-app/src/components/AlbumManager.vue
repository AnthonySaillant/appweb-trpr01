<script setup lang="ts">
import { ref } from "vue";
import AddAlbumForm from "./AddAlbumForm.vue";
import type { Album } from "../types";

const albums = ref<Album[]>([]);

const handleAlbumsUpdate = (updatedAlbums: Album[]) => {
  albums.value = [...updatedAlbums];
};
</script>

<template>
  <AddAlbumForm @update:albums="handleAlbumsUpdate" />

  <div class="container mt-4">
    <h2>Liste d'Albums</h2>
    <ul class="list-group">
      <li v-for="album in albums" :key="album.id" class="list-group-item">
        {{ album.name }} - {{ album.description }} - Quantité:
        {{ album.stock }} - Prix: ${{ album.price }}
      </li>
    </ul>
  </div>
</template>

<style scoped></style>
