<template>
    <div 
      class="modal" 
      v-if="show" 
      @click="hideModal"
    >
      <div 
        class="modal-content" 
        @click.stop
      >
        <slot></slot>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'modal-window',
    props: {
      show: {
        type: Boolean,
        default: false,
      },
    },
    methods: {
      hideModal() {
        this.$emit('update:show', false);
      },
    },
  };
  </script>
  
  <style scoped>
  .modal {
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background-color: rgba(0, 0, 0, 0.5);
    position: fixed;
    display: flex;
    align-items: center;
    justify-content: center;
    animation: fadeIn 0.3s ease;
  }
  .modal-content {
    background-color: white;
    border-radius: 12px;
    padding: 20px;
    min-width: 300px;
    min-height: 150px;
    animation: slideIn 0.3s ease;
  }
  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
  @keyframes slideIn {
    from {
      transform: translateY(-20px);
    }
    to {
      transform: translateY(0);
    }
  }
  </style>
  