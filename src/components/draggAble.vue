<template>
  <draggable v-model="todosDrag"></draggable>
</template>
<script>
import { VueDraggableNext } from 'vue-draggable-next'
export default {
  name:'draggable'
};
</script>