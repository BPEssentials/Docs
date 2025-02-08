<template>
  <p class="text">
    <slot></slot>
  </p>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'MainText'
});
</script>

<style scoped>
.text {
  margin: 0 5%;
  font-size: 1.4em;
}
</style>