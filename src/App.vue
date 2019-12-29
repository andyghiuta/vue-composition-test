<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld
      msg="Welcome to Your Vue.js App"
      :autoFocus="autoFocus"
      :select="select"
      v-model="info"
    />
    <h2>Your info: Name: {{ name }}; Age: {{ age }}</h2>
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
    info: 'Provide your name-18',
    name: 'Provide your name',
    age: 18,
    autoFocus: true,
    select: true,
  }),
  watch: {
    info() {
      const [name, age] = this.info.split('-');
      this.name = name;
      this.age = parseInt(age, 10);
    },
  },
  methods: {
    reset() {
      this.info = 'Provide your name-18';
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
