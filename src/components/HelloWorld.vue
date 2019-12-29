<template>
  <div
    ref="el"
    class="hello">
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
import {
  ref, reactive, toRefs, computed, watch, onMounted,
} from '@vue/composition-api';

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
  setup(props, context) {
    // reactive properties
    const changes = ref(0);
    const info = reactive(splitInfo(props.value));

    // computed properties
    const personInfo = computed(() => `${normalizeName(info.name)}-${info.age}`);

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
    const setFocus = () => {
      if (props.autoFocus) {
        context.refs.el.querySelector('input').focus();
      }
    };
    const setSelect = () => {
      if (props.select) {
        setTimeout(() => {
          context.refs.el.querySelector('input').select();
        }, 100);
      }
    };

    // define watches
    // props, refs and reactive objects can be watched for changes
    // can watch a getter function
    watch(() => props.autoFocus, setFocus);
    watch(() => props.select, setSelect);
    // optionally, can have be lazy (won't run on component initialize)
    // defaults to false, contrary to how watches work in Vue 2
    watch(() => props.value, (outsideValue) => {
      Object.assign(info, splitInfo(outsideValue));
    }, { lazy: true });
    // watch a specific ref (computed)
    watch(personInfo, () => {
      setChanges();
      context.emit('input', personInfo.value);
    });
    // lifecycle hooks
    onMounted(() => {
      setFocus();
      setSelect();
    });
    // return the state with the reactive properties & methods
    // each property must be a ref
    return {
      // return properties
      // changes is a ref, can be returned as such
      changes,
      // to convert a reactive object to a plain object with refs, use toRefs
      ...toRefs(info),
      // return computed properties
      personInfo,
      // return methods
      increaseAge,
      decreaseAge,
      setChanges,
      setFocus,
      setSelect,
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
