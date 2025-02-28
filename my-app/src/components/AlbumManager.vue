<script setup lang="ts">
import { ref } from "vue";
import AddAlbumForm from "./AddAlbumForm.vue";
import type { Album } from "../types";
import AlbumItem from "./AlbumItem.vue";

const albums = ref<Album[]>([]);

const handleNewAlbum = (newAlbum: Album) => {
  albums.value.push(newAlbum);
};

const handleDeleteAlbum = (albumId: number) => {
  albums.value = albums.value.filter((album) => album.id !== albumId);
};
</script>

<template>
  <div class="container mt-4">
    <h2>Liste d'Albums</h2>
    <ul class="list-group">
      <li v-for="album in albums" :key="album.id" class="list-group-item">
        <AlbumItem
          :album="album"
          :key="album.id"
          @deleteAlbum="handleDeleteAlbum"
        />
      </li>
    </ul>
  </div>
  <AddAlbumForm @update:albums="handleNewAlbum" />
</template>

<style scoped></style>
