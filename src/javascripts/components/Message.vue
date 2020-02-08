<template>
  <div class="wrapMessage">
    <div v-for="data in $props.message">
      <div v-if="data.status === 'send'" class="message">
        <div class="name">{{ data.name }}</div>
        <div class="text">{{ data.text }}</div>
      </div>
      <div v-else-if="data.status === 'connect'" class="messageSys">
        <div class="info">ユーザが参加しました ({{ data.text }})</div>
      </div>
      <div v-else-if="data.status === 'disconnect'" class="messageSys">
        <div class="info">ユーザが退出しました ({{ data.text }})</div>
      </div>
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
        status: VueTypes.string,
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
  width: 95%;
  box-sizing: border-box;
  background: #fff;
  margin: 5px 0;
  padding: 10px;
  border: 1px solid #aaa;
}

.messageSys {
  width: 95%;
  box-sizing: border-box;
  background: #aaa;
  margin: 5px 0;
  padding: 5px;
  border: 1px solid #aaa;
}

.text {
  padding-top: 5px;
  padding-left: 10px;
}

.info {
  font-size: 14px;
  color: #333;
  background: #aaa;
}
</style>
