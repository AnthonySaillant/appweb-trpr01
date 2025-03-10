<script setup lang="ts">
import { ref, onMounted } from "vue";
import AddAlbumForm from "./AddAlbumForm.vue";
import EditAlbumForm from "./EditAlbumForm.vue";
import AlbumItem from "./AlbumItem.vue";
import DeleteConfirmation from "./DeleteConfirmation.vue";
import OutOfStock from "./OutOfStock.vue";
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
  showDeleteConfirmation.value = true;
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

//chatgpt pour tout ce qui est en rapport au csv
const exportToCSV = () => {
  const headers = ["ID", " Title", " Artist", " Genre", " Release Date"];
  const rows = albums.value.map((album) => [
    album.id,
    album.name,
    album.description,
    album.stock,
    album.price,
  ]);

  let csvContent =
    "data:text/csv;charset=utf-8," +
    headers.join(",") +
    "\n" +
    rows.map((row) => row.join(", ")).join("\n");

  const encodedUri = encodeURI(csvContent);
  const link = document.createElement("a");
  link.setAttribute("href", encodedUri);
  link.setAttribute("download", "albums.csv");
  link.click();
};

onMounted(() => {
  albums.value = [
    {
      id: nextId++,
      name: "Master of Puppets",
      description: "Metallica",
      stock: 0,
      price: 12.99,
    },
    {
      id: nextId++,
      name: "The Number of the Beast",
      description: "Iron Maiden",
      stock: 5,
      price: 10.99,
    },
    {
      id: nextId++,
      name: "Painkiller",
      description: "Judas Priest",
      stock: 0,
      price: 14.99,
    },
    {
      id: nextId++,
      name: "Reign in Blood",
      description: "Slayer",
      stock: 3,
      price: 13.49,
    },
  ];
});
</script>

<!-- Bootstrap par chatgpt -->
<template>
  <div class="container mt-4">
    <div class="text-center mb-4">
      <img
        src="/src/assets/TP1-SongManager-Logo.webp"
        alt="Image du Site"
        class="d-block mx-auto mb-4"
        style="width: 150px; height: auto"
      />
    </div>

    <div class="d-flex justify-content-end mb-3">
      <button
        class="btn btn-primary me-3 d-flex align-items-center"
        @click="exportToCSV"
        style="height: 38px"
      >
        Exporter CSV
      </button>

      <button
        class="btn mb-3 d-flex align-items-center"
        :class="showAddForm ? 'btn-danger' : 'btn-success'"
        @click="
          showAddForm = !showAddForm;
          duplicateAlbumData = null;
        "
        style="height: 38px"
      >
        {{ showAddForm ? "Annuler l'addition" : "Ajouter un nouvel album" }}
      </button>
    </div>

    <AddAlbumForm
      v-show="showAddForm"
      @update:albums="handleNewAlbum"
      :prefilledAlbum="duplicateAlbumData"
    />

    <EditAlbumForm
      v-if="selectedAlbum"
      :album="selectedAlbum"
      @update:album="handleUpdateAlbum"
    />

    <OutOfStock :albums="albums" />

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

    <DeleteConfirmation
      v-if="showDeleteConfirmation"
      :albumId="albumToDelete"
      @deleteConfirmed="handleDeleteConfirmed"
      @close="showDeleteConfirmation = false"
    />
  </div>
</template>

<style scoped></style>
