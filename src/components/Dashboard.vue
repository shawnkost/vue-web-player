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
  methods: {
    async login() {
      const code = this.dashCode;
      try {
        const res = await axios.post("http://localhost:3001/login", {
          code,
        });
        this.accessToken = res.data.accessToken;
        this.refreshToken = res.data.refreshToken;
        this.expiresIn = res.data.expiresIn;
        window.history.pushState({}, null, "/");
      } catch {
        window.location = "/";
      }
    },
  },
  created() {
    this.login();
  },
};
</script>

<style scoped></style>
