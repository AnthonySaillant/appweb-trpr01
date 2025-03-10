<script setup lang="ts">
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  albumId: {
    type: Number,
    required: true,
  },
});

const emit = defineEmits();

const confirmDeletion = () => {
  emit("deleteConfirmed", props.albumId);
};
</script>

<!-- Bootstrap par chatgpt -->
<template>
  <div
    class="modal fade"
    id="deleteModal"
    tabindex="-1"
    aria-labelledby="deleteModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="deleteModalLabel">
            Confirmer la suppression
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
          ></button>
        </div>
        <div class="modal-body">
          <p>Voulez-vous vraiment supprimer cet album ?</p>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            Annuler
          </button>
          <button
            type="button"
            class="btn btn-danger"
            @click="confirmDeletion"
            data-bs-dismiss="modal"
          >
            Supprimer
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
