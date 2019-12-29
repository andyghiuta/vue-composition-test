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
import { ref, reactive, toRefs } from '@vue/composition-api';

const splitInfo = (info) => {
  const [name, age] = info.split('-');
  return { name, age: parseInt(age, 10) };
};

const normalizeName = name => name.toUpperCase();

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    value: String,
    autoFocus: Boolean,
    select: Boolean,
  },
  computed: {
    personInfo() {
      return `${normalizeName(this.name)}-${this.age}`;
    },
  },
  watch: {
    value(outsideValue) {
      Object.assign(this, splitInfo(outsideValue));
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
  },
  setup(props) {
    // reactive properties
    const changes = ref(0);
    const info = reactive(splitInfo(props.value));

    // methods
    const increaseAge = () => {
      info.age += 1;
    };
    const decreaseAge = () => {
      info.age -= 1;
    };
    const setChanges = () => {
      // refs need to be accessed with the value property
      changes.value += 1;
    };
    // return the state with the reactive properties & methods
    // each property must be a ref
    return {
      // return properties
      // changes is a ref, can be returned as such
      changes,
      // to convert a reactive object to a plain object with refs, use toRefs
      ...toRefs(info),
      // return methods
      increaseAge,
      decreaseAge,
      setChanges,
    };
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
