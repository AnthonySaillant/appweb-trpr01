<script setup lang="ts">
import { defineProps, defineEmits, ref } from "vue";

const props = defineProps({
  albumId: {
    type: Number,
    required: true,
  },
});

const emit = defineEmits();

const show = ref(true);

const close = () => {
  show.value = false;
  emit("close");
};

const confirmDeletion = () => {
  emit("deleteConfirmed", props.albumId);
  close();
};
</script>

<template>
  <div
    class="modal fade show"
    tabindex="-1"
    v-show="show"
    @click.self="close"
    style="display: block"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">Confirmer la suppression</h5>
          <button type="button" class="btn-close" @click="close"></button>
        </div>
        <div class="modal-body">
          <p>Voulez-vous vraiment supprimer cet album?</p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" @click="close">
            Annuler
          </button>
          <button type="button" class="btn btn-danger" @click="confirmDeletion">
            Supprimer
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal.fade {
  background-color: rgba(0, 0, 0, 0.5);
}
</style>
