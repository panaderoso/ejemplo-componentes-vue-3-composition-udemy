<script setup>
defineProps(["inicio", "fin", "maxLength"]);

const emit = defineEmits(["next", "prev"]);

const activarSiguiente = () => {
  console.log("le dista a Siguiente!");
  emit("next");
};
</script>

<template>
  <div class="btn-group mb-2" role="group" aria-label="Basic example">
    <button
      @click="emit('prev')"
      type="button"
      class="btn btn-outline-primary"
      :disabled="inicio <= 0"
    >
      Pervius {{ inicio }}
    </button>
    <button
      @click="activarSiguiente"
      type="button"
      class="btn btn-outline-primary"
      :disabled="fin >= maxLength"
    >
      Next {{ fin }}
    </button>
  </div>
</template>
