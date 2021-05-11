<template>
  <div class="login">
    <img
      src="https://music-b26f.kxcdn.com/wp-content/uploads/2017/06/635963274692858859903160895_spotify-logo-horizontal-black.jpg"
      alt="Spotify logo"
    />
    <a
      href="https://accounts.spotify.com/authorize?client_id=58c63d6c452940378c16d1e4a5a65ee0&response_type=code&redirect_uri=http://localhost:8080&scope=streaming%20user-read-email%20user-read-private%20user-library-read%20user-library-modify%20user-read-playback-state%20user-modify-playback-state"
      >LOGIN WITH SPOTIFY</a
    >
  </div>
</template>

<script>
export default {
  name: "Login",
};
</script>

<style scoped>
.login {
  display: grid;
  place-items: center;
  height: 100vh;
  background-color: black;
}

.login > img {
  width: auto;
  height: 200px;
}

.login > a {
  padding: 20px;
  border-radius: 99px;
  font-weight: 600;
  color: white;
  text-decoration: none;
  background-color: #1db954;
}
</style>
