<template>
  <div id="app">
    <h2 v-html="title"></h2>
    <input v-model="newItem" @keyup.enter="addNew" type="text" />
    <ul>
      <li v-for="(item, index) in items" 
          :key="item.id" 
          v-bind:class="['li-'+index, {'finished': item.isFinished}]"
          v-on:click="toggle(item)">
        {{ item.label }}
      </li>
    </ul>
    <p>child say: {{ childWords }}</p>
    <HelloWorld :newtem="newItem" v-on:tell-some="tellSome"></HelloWorld>

  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Store from './store.js'
// console.log(Store)

export default {
  data () {
    return {
      title: '<p>Enter some you want...</p>',
      items: Store.fetch(),
      newItem: '',
      childWords: ''
    }
  },
  components: { HelloWorld },
  watch: {
    items: {
      handler (items) {
        // console.log(items)
        Store.save(items);
      },
      deep: true
    }
  },
  methods: {
    toggle (event) {
      // console.log(event)
      event.isFinished = !event.isFinished;
    },
    addNew () {
      // console.log(this.newItem)
      this.items.unshift({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = '';
    },
    tellSome (msg) {
      this.childWords = msg;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.finished {
  text-decoration: underline;
}
</style>
