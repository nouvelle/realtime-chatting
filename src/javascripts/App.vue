<template>
  <div class="wrap">
    <p class="header">
      <img class="logo" src="../images/icon.png" alt="キャラクター" />
    </p>
    <form @submit="onSubmit">
      <div class="wrapInput">
        <div class="inputTitle">Name</div>
        <input v-model="$data.name" type="text" />
      </div>
      <div class="wrapInput">
        <div class="inputTitle">Message</div>
        <input v-model="$data.text" type="text" />
      </div>
      <button type="submit" class="btn">送信</button>
    </form>
    <p class="clientNum">現在の接続数：{{ $data.clientNum }}</p>
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
      clientNum: 0,
      inputData: [],
    };
  },
  created() {
    socket.on('connect', () => {
      console.log('connected!');
    });

    // コネクト時
    socket.on('connected', data => {
      console.log('connected', data);
      this.clientNum = data.clientNum;
      this.$data.inputData.unshift({
        status: 'connect',
        name: 'system',
        text: data.id,
        date: data.date,
      });
    });
    // disコネクト時
    socket.on('disconnected', data => {
      console.log('disconnected', data);
      this.clientNum = data.clientNum;
      this.$data.inputData.unshift({
        status: 'disconnect',
        name: 'system',
        text: data.id,
        date: data.date,
      });
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
          status: 'send',
          name: this.$data.name,
          text: this.$data.text,
          date: new Date(),
        });
      }
      this.$data.text = '';
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  font-family: 'Ubuntu', sans-serif;
  outline: none;
}

.wrap {
  margin: 10px;
}

.logo {
  width: 80px;
}

.header {
  margin: 10px auto;
  text-align: center;
}

.wrapInput {
  margin-bottom: 5px;
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
  width: 90%;
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

.clientNum {
  margin: 0;
  font-size: 14px;
  color: #333;
  text-align: right;
}
</style>
