<template>
  <div class="pagedivider"></div>
</template>

<script>
export default {
  name: 'PageDivider'
}
</script>

<style lang="scss">
@use "sass:map";
@use "@/scss/global.scss" as *;

.pagedivider {
  height: 3px;
  width: 100%;
  background-color: map.get($colors, divider);
}
</style>
