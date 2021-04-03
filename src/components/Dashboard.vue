<template>
  <div>
    {{ code }}
  </div>
</template>

<script>
import axios from "axios";

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
      dashCode: this.code,
    };
  },
  created() {
    const code = this.$data.dashCode;
    axios
      .post("http://localhost:3001/login", {
        code,
      })
      .then((res) => {
        console.log(res.data);
      })
      .catch((err) => {
        console.log("failed", err);
      });
  },
};
</script>

<style scoped></style>
