<script setup lang="ts">
import { ref, defineProps, computed, onMounted, nextTick } from "vue";
import type { Album } from "../types";

const props = defineProps({
  albums: {
    type: Array as () => Album[],
    required: true,
  },
});

const showAlert = ref(true);

const outOfStockAlbums = computed(() => {
  return props.albums.filter((album) => album.stock === 0);
});

// Auto-dismiss alert after 5 seconds
onMounted(() => {
  nextTick(() => {
    if (outOfStockAlbums.value.length > 0) {
      setTimeout(() => {
        showAlert.value = false;
      }, 5000); // Hide alert after 5 seconds
    }
  });
});

const closeAlert = () => {
  showAlert.value = false; // Manually close the alert
};
</script>

<template>
  <!-- Out of Stock Albums Section -->
  <div
    v-if="showAlert && outOfStockAlbums.length > 0"
    class="alert alert-danger alert-dismissible fade show"
    role="alert"
  >
    <h4 class="alert-heading">Albums en rupture de stock</h4>
    <ul class="list-group">
      <li
        v-for="album in outOfStockAlbums"
        :key="album.id"
        class="list-group-item"
      >
        {{ album.name }} - {{ album.description }}
      </li>
    </ul>
    <button
      type="button"
      class="btn-close"
      aria-label="Close"
      @click="closeAlert"
    ></button>
  </div>
</template>
