<template>
  <Dashboard v-if="code" />
  <Login v-else />
</template>

<script>
import Login from "./components/Login"
import Dashboard from "./components/Dashboard"

export default {
  name: 'App',
  components: {
    Login,
    Dashboard
  },
  data() {
    return {
      code: new URLSearchParams(window.location.search).get("code")
    }
  }
}
</script>

<style>
</style>
