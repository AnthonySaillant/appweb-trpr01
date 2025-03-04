<script setup lang="ts">
import { defineEmits, ref } from "vue";

const emit = defineEmits<{
  (event: "searchQuery", query: string): void;
}>();

const searchQuery = ref("");

const onSearch = () => {
  emit("searchQuery", searchQuery.value);
};
</script>

<template>
  <div class="mb-4">
    <input
      type="text"
      class="form-control"
      v-model="searchQuery"
      @input="onSearch"
      placeholder="Search for an album..."
    />
  </div>
</template>

<style scoped></style>
