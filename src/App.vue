<template>
  <div id="app">
    <component :is="layout">
      <router-view :layout.sync="layout" />
    </component>
  </div>
</template>
<script>
export default {
  name: 'App',
  components: {
  },
  data () {
    return {
      layout: 'div'
    }
  }
}
</script>
