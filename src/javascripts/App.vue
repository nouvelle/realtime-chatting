<template>
  <div>
    <p>
      <img class="logo" src="../images/logo.jpg" alt="ロゴ" />
      <span class="sample">サンプルコード</span>
    </p>
    <form @submit="onSubmit">
      Name:
      <input v-model="$data.name" type="text" />
      Message:
      <input v-model="$data.text" type="text" />
      <button type="submit">送信</button>
    </form>
    <Message :message="$data.inputData" />
  </div>
</template>

<script>
import socket from './utils/socket';

// components
import Message from './components/Message.vue';

export default {
  components: {
    Message,
  },
  data() {
    return {
      name: '',
      text: '',
      inputData: [],
    };
  },
  created() {
    socket.on('connect', () => {
      console.log('connected!');
    });

    socket.on('send', message => {
      this.$data.inputData.unshift(message);
    });
  },
  methods: {
    /**
     * Enterボタンを押したとき
     */
    onSubmit(e) {
      e.preventDefault();
      console.log(this.$data.name, this.$data.text);
      socket.json.emit('send', {
        name: this.$data.name,
        text: this.$data.text,
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.logo {
  width: 40px;
}

.sample {
  color: $red;
}
</style>
