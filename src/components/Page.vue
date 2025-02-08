<template>
  <div>
    <Divider v-show="divider"/>
    <div :class="{ 'full': !fitContent, 'fit-content': fitContent }">
      <MainTitle :sub="sub" :small="small" :xs="xs" v-show="title">{{title}}</MainTitle>
      <slot></slot>
    </div>
  </div>
</template>

<script>
import { defineComponent, toRefs } from 'vue';
import MainTitle from './MainTitle.vue';
import Divider from './PageDivider.vue';

export default defineComponent({
  name: 'Page',
  components: {
    MainTitle,
    Divider
  },
  props: {
    title: String,
    small: Boolean,
    sub: Boolean,
    xs: Boolean,
    fitContent: {
      type: Boolean,
      default: true
    },
    divider: {
      type: Boolean,
      default: true
    }
  },
  setup(props) {
    return { ...toRefs(props) };
  }
});
</script>

<style>
.fit-content {
  padding-bottom: 30px;
}
.full {
  height: 100vh;
}
</style>
