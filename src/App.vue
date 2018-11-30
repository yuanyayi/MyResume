<template>
  <div id="app">
    <ResumeEditor/>
  </div>
</template>

<script>
import ResumeEditor from 'components/ResumeEditor/ResumeEditor'

export default {
  name: 'app',
  components: {
    ResumeEditor
  }
}
</script>

<style lang="less">
* {
  margin: 0;
  padding: 0;
}

ul,ol,dl {
  list-style: none;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
