<template>
  <button>
    <slot></slot>
  </button>
</template>

<script lang="ts">
export default {
  name: 'app-button',
};
</script>

<style scoped>

button {
  width: 100%;
  padding: 12px;
  background-color: slateblue;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:active {
  transform: translate(1px, 1px);
}

button:disabled {
  background-color: #95b8e2;
  cursor: not-allowed;
}

</style>
