<template>
    <div v-if="visible" class="confirm-dialog">
      <div class="confirm-dialog-content">
        <p>{{ message }}</p>
        <button @click="confirmer">Confirmer</button>
        <button @click="annuler">Annuler</button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      message: String,
      onConfirm: Function,
      onCancel: Function,
    },
    data() {
      return {
        visible: true,
      };
    },
    methods: {
      confirmer() {
        this.visible = false;
        this.onConfirm();
      },
      annuler() {
        this.visible = false;
        this.onCancel();
      },
    },
  };
  </script>
  
  <style scoped>
  .confirm-dialog {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
  }
  
  .confirm-dialog-content {
    background: white;
    padding: 20px;
    border-radius: 4px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
    text-align: center;
  }
  
  .confirm-dialog button {
    margin: 0 10px;
  }
  </style>
  