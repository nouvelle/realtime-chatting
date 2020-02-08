<template>
  <div class="wrap">
    <p class="header">
      <img class="logo" src="../images/icon.png" alt="キャラクター" />
    </p>
    <form @submit="onSubmit">
      <div class="wrapInput">
        <label class="inputTitle">Name</label>
        <input v-model="$data.name" type="text" />
      </div>
      <div class="wrapInput">
        <label class="inputTitle">Message</label>
        <input v-model="$data.text" type="text" />
      </div>
      <button type="submit" class="btn">送信</button>
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
      if (this.$data.name && this.$data.text) {
        console.log(this.$data.name, this.$data.text);
        socket.json.emit('send', {
          name: this.$data.name,
          text: this.$data.text,
        });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  outline: none;
}

.wrap {
  margin: 20px;
}

.logo {
  width: 80px;
}

.header {
  margin: 10px auto;
  text-align: center;
}

.wrapInput {
  height: 35px;
}

.inputTitle {
  display: inline-block;
  width: 80px;
}

form {
  margin-left: 5%;
}

input {
  padding: 5px;
  border: solid 1px #ccc;
  width: 250px;
}

input:focus {
  border: solid 1px #eea34a;
}

.btn {
  display: inline-block;
  padding: 0.5em 1em;
  text-decoration: none;
  background: #668ad8;
  color: #fff;
  border-bottom: solid 4px #627295;
  border-radius: 3px;
}

.btn:active {
  transform: translateY(4px);
  border-bottom: none;
}

.sample {
  color: $red;
}
</style>
