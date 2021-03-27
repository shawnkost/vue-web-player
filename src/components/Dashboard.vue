<template>
  <div>
    {{ code }}
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "Dashboard",
  props: {
    code: String,
  },
  data() {
    return {
      accessToken: "",
      refreshToken: "",
      expiresIn: "",
      dashCode: this.code
    }
  },
  created(dashCode) {
    axios.post('http://localhost:3001/login', {
        dashCode,
      }).then((res) => {
        console.log(res.data)
      }).catch(() => {
        console.log("failed")
      })
  }
};
</script>

<style scoped></style>
