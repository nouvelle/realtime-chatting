<template>
  <div>
    <div v-for="data in $props.message">{{ data.name}}: {{data.text}}</div>
  </div>
</template>

<script>
import VueTypes from 'vue-types';

export default {
  name: 'Message',
  props: {
    message: VueTypes.arrayOf(
      VueTypes.shape({
        text: VueTypes.string,
        name: VueTypes.string,
      })
    ),
  },
};
</script>

<style lang="scss" scoped>
//
</style>
