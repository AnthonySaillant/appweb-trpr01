<script setup lang="ts">
import { defineProps, defineEmits, ref, watch } from "vue";
import type { Album } from "../types";
const props = defineProps<{ prefilledAlbum?: Album | null }>();
const emit = defineEmits<{ (event: "update:albums", album: Album): void }>();

const stringErrorMessage = "Ce champ ne peut pas être vide.";
const numberMustBePositiveErrorMessage = "La valeur doit être au dessus de 0.";

const album = ref<Album>({
  id: 0,
  name: "",
  description: "",
  stock: 0,
  price: 0,
});

const errors = ref({
  name: false,
  description: false,
  stock: false,
  price: false,
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

const validateForm = () => {
  errors.value.name = album.value.name.trim() === "";
  errors.value.description = album.value.description.trim() === "";
  errors.value.stock = album.value.stock < 0;
  errors.value.price = album.value.price < 0;

  return !Object.values(errors.value).includes(true);
};

const addAlbum = () => {
  if (!validateForm()) return;

  emit("update:albums", { ...album.value });
  album.value = { id: 0, name: "", description: "", stock: 0, price: 0 };
};
</script>

<template>
  <div class="card p-3 mb-4">
    <!-- Added mb-4 for bottom margin -->
    <h3>{{ prefilledAlbum ? "Album Dupliqué" : "Ajouter un Nouvel Album" }}</h3>
    <form @submit.prevent="addAlbum" class="needs-validation">
      <div class="mb-2">
        <label class="form-label">Nom:</label>
        <input
          v-model="album.name"
          class="form-control"
          :class="{ 'is-invalid': errors.name }"
        />
        <div v-if="errors.name" class="invalid-feedback">
          {{ stringErrorMessage }}
        </div>
      </div>

      <div class="mb-2">
        <label class="form-label">Description:</label>
        <input
          v-model="album.description"
          class="form-control"
          :class="{ 'is-invalid': errors.description }"
        />
        <div v-if="errors.description" class="invalid-feedback">
          {{ stringErrorMessage }}
        </div>
      </div>

      <div class="mb-2">
        <label class="form-label">Quantité:</label>
        <input
          type="number"
          v-model="album.stock"
          class="form-control"
          :class="{ 'is-invalid': errors.stock }"
        />
        <div v-if="errors.stock" class="invalid-feedback">
          {{ numberMustBePositiveErrorMessage }}
        </div>
      </div>

      <div class="mb-2">
        <label class="form-label">Prix:</label>
        <input
          type="number"
          v-model="album.price"
          class="form-control"
          :class="{ 'is-invalid': errors.price }"
        />
        <div v-if="errors.price" class="invalid-feedback">
          {{ numberMustBePositiveErrorMessage }}
        </div>
      </div>

      <button type="submit" class="btn btn-primary">Enregistrer</button>
    </form>
  </div>
</template>

<style scoped></style>
