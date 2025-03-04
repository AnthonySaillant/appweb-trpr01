<script setup lang="ts">
import { ref } from "vue";
import AddAlbumForm from "./AddAlbumForm.vue";
import EditAlbumForm from "./EditAlbumForm.vue";
import AlbumItem from "./AlbumItem.vue";
import DeleteConfirmation from "./DeleteConfirmation.vue";
import type { Album } from "../types";

const albums = ref<Album[]>([]);
const showAddForm = ref(false);
const selectedAlbum = ref<Album | null>(null);
const duplicateAlbumData = ref<Album | null>(null);
const showDeleteConfirmation = ref(false);
const albumToDelete = ref<number | null>(null);
let nextId = 1;

const handleNewAlbum = (newAlbum: Album) => {
  newAlbum.id = nextId++;
  albums.value.push(newAlbum);
  showAddForm.value = false;
  duplicateAlbumData.value = null;
};

const handleDeleteAlbum = (albumId: number) => {
  albumToDelete.value = albumId;
  showDeleteConfirmation.value = true; // Show the confirmation modal
};

const handleDeleteConfirmed = (albumId: number) => {
  albums.value = albums.value.filter((album) => album.id !== albumId);
  showDeleteConfirmation.value = false;
  albumToDelete.value = null;
};

const handleEditAlbum = (album: Album) => {
  selectedAlbum.value = album;
};

const handleUpdateAlbum = (updatedAlbum: Album) => {
  const index = albums.value.findIndex((album) => album.id === updatedAlbum.id);
  if (index !== -1) {
    albums.value[index] = updatedAlbum;
  }
  selectedAlbum.value = null;
};

const handleDuplicateAlbum = (album: Album) => {
  duplicateAlbumData.value = { ...album, id: nextId };
  showAddForm.value = true;
};
</script>

<template>
  <div class="container mt-4">
    <!-- Image at the top -->
    <div class="text-center mb-4">
      <img
        src="/src/assets/TP1-SongManager-Logo.webp"
        alt="Image du Site"
        class="d-block mx-auto mb-4"
        style="width: 150px; height: auto"
      />
    </div>

    <!-- Add New Album Button -->
    <div class="d-flex justify-content-end">
      <button
        class="btn mb-3"
        :class="showAddForm ? 'btn-danger' : 'btn-success'"
        @click="
          showAddForm = !showAddForm;
          duplicateAlbumData = null;
        "
      >
        {{ showAddForm ? "Annuler" : "Ajouter un nouvel album" }}
      </button>
    </div>

    <!-- Add New Album Form -->
    <AddAlbumForm
      v-show="showAddForm"
      @update:albums="handleNewAlbum"
      :prefilledAlbum="duplicateAlbumData"
    />

    <!-- Edit Album Form -->
    <EditAlbumForm
      v-if="selectedAlbum"
      :album="selectedAlbum"
      @update:album="handleUpdateAlbum"
    />

    <!-- Album List Section -->
    <h2>Liste d'Albums</h2>
    <ul class="list-group">
      <li v-for="album in albums" :key="album.id" class="list-group-item">
        <AlbumItem
          :album="album"
          @deleteAlbum="handleDeleteAlbum"
          @editAlbum="handleEditAlbum"
          @duplicateAlbum="handleDuplicateAlbum"
        />
      </li>
    </ul>

    <!-- Delete Confirmation Modal (Bootstrap) -->
    <DeleteConfirmation
      v-if="showDeleteConfirmation"
      :albumId="albumToDelete"
      @deleteConfirmed="handleDeleteConfirmed"
      @close="showDeleteConfirmation = false"
    />
  </div>
</template>
