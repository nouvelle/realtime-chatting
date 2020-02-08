<template>
  <div class="wrapMessage">
    <div v-for="data in $props.message" class="message">
      <div class="name">{{ data.name }}</div>
      <div class="text">{{ data.text }}</div>
    </div>
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
.wrapMessage {
  width: 100%;
  margin: 15px 10px;
}

.message {
  width: 90%;
  background: #fff;
  margin: 5px 0;
  padding: 15px 10px;
  border: 1px solid #aaa;
}

.text {
  padding-top: 5px;
  padding-left: 10px;
}
</style>
