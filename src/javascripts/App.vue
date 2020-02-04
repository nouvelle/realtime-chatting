<template>
  <div>
    <p>
      <img class="logo" src="../images/logo.jpg" alt="ロゴ" />
      <span class="sample">サンプルコード</span>
    </p>
    <MyComponent :message="$data.message" />
    <form @submit="onSubmit">
      Name:
      <input v-model="$data.inputData.name" type="text" />
      Message:
      <input v-model="$data.inputData.text" type="text" />
      <button type="submit">送信</button>
    </form>
  </div>
</template>

<script>
import socket from './utils/socket';

// components
import MyComponent from './components/MyComponent.vue';

export default {
  components: {
    MyComponent,
  },
  data() {
    return {
      message: '',
      inputData: {
        text: '',
        name: '',
      },
    };
  },
  created() {
    socket.on('connect', () => {
      console.log('connected!');
    });

    socket.on('send', message => {
      console.log(message);
      this.$data.message = message;
    });
  },
  methods: {
    /**
     * Enterボタンを押したとき
     */
    onSubmit(e) {
      e.preventDefault();
      console.log(this.$data.inputData);
      socket.emit('send', this.$data.inputData);
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
