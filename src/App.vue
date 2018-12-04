<template>
  <div id="app">
    <ResumeEditor/>
  </div>
</template>

<script>
import ResumeEditor from 'sideProjects/ResumeEditor/ResumeEditor'

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

html,body {
  height: 100%;
}

ul,ol,dl {
  list-style: none;
}

#app {
  // font-family: 'Avenir', Helvetica, Arial, sans-serif;
  font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100%;
}

</style>
