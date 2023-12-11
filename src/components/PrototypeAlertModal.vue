<!-- PrototypeAlertModal.vue -->
<template>
  <div class="modal-overlay" v-if="isVisible">
    <div class="modal">
      <h2>⚠️ Attention ⚠️</h2>
      <p>Ceci est un prototype et la version finale peut différer de ce que vous voyez ici.</p>
      <p>Comme il s'agit d'un prototype, il est possible qu'il contienne des bugs.</p>
      <button @click="closeModal">OK</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isVisible: true,
    };
  },
  methods: {
    closeModal() {
      this.isVisible = false;
    }
  }
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000; /* Valeur élevée pour s'assurer qu'il apparaît au-dessus des autres éléments */
}

.modal {
  background: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
  z-index: 1001; /* Encore plus élevé pour s'assurer que le contenu du modal est au-dessus de l'overlay */
}

button {
  background: #009fdb;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}
</style>

