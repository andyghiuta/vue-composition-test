<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld
      msg="Welcome to Your Vue.js App"
      :autoFocus="autoFocus"
      :select="select"
      v-model="name"
    />
    <h2>Your name is: {{ name }}</h2>
    <button type="button" @click="reset">Reset</button>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';

export default {
  name: 'app',
  components: {
    HelloWorld,
  },
  data: () => ({
    name: 'Provide your name',
    autoFocus: true,
    select: true,
  }),
  methods: {
    reset() {
      this.name = 'Provide your name';
      this.select = false;
      this.autoFocus = false;
      this.$nextTick(() => {
        this.select = true;
        this.autoFocus = true;
      });
    },
  },
};
</script>

<style lang="less">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
