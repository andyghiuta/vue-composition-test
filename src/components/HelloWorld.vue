<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <label>Enter your name: </label>
    <input type="text" v-model="name" /><br>
    <label>Set your age: </label>
    <button type="button" @click="decreaseAge"> - </button>
    <span> {{age}} </span>
    <button type="button" @click="increaseAge"> + </button>
    <p><small>You made {{changes}} changes to your info</small></p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    value: String,
    autoFocus: Boolean,
    select: Boolean,
  },
  data() {
    const info = this.splitInfo(this.value);
    return {
      ...info,
      changes: 0,
    };
  },
  computed: {
    personInfo() {
      return `${this.normalizeName(this.name)}-${this.age}`;
    },
  },
  watch: {
    value(outsideValue) {
      Object.assign(this, this.splitInfo(outsideValue));
    },
    personInfo() {
      this.setChanges();
      this.$emit('input', this.personInfo);
    },
    autoFocus() {
      this.setFocus();
    },
    select() {
      this.setSelect();
    },
  },
  mounted() {
    this.setFocus();
    this.setSelect();
  },
  methods: {
    setChanges() {
      this.changes += 1;
    },
    setFocus() {
      if (this.autoFocus) {
        this.$el.querySelector('input').focus();
      }
    },
    setSelect() {
      if (this.select) {
        this.$el.querySelector('input').select();
      }
    },
    normalizeName(name) {
      return name.toUpperCase();
    },
    increaseAge() {
      this.age += 1;
    },
    decreaseAge() {
      this.age -= 1;
    },
    splitInfo(info) {
      const [name, age] = info.split('-');
      return { name, age: parseInt(age, 10) };
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
