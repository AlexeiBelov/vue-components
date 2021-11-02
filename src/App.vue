<template>
  <div id="app">
    <SimpleSelect
        :options="[{ label: 'Текст', choice: true },{ label: 'Длинный текст', choice: false },{ label: 'Очень длинный длинный длинный текст', choice: false },{ label: 'Текст', choice: false },{ label: 'Текст', choice: false },{ label: 'Текст', choice: false },{ label: 'Текст', choice: false },{ label: 'ttttТекст', choice: false },]"
        :disabled="false"
    />
  </div>
</template>

<script>
import SimpleSelect from './components/select/SimpleSelect.vue'

export default {
  name: 'App',
  components: {
    SimpleSelect
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
