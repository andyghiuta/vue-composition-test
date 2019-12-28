<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <label>Enter your name:</label>
    <input type="text" v-model="myName" />
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
    return {
      myName: this.value,
    };
  },
  computed: {
    normalizedName() {
      return this.normalizeName(this.myName);
    },
  },
  watch: {
    value(outsideName) {
      this.myName = outsideName;
    },
    normalizedName() {
      this.$emit('input', this.normalizedName);
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
    normalizeName(myName) {
      return myName.toUpperCase();
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
