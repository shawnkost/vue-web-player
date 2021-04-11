<template>
  <div class="container d-flex flex-column py-2" :style="{ height: '100vh' }">
    <input
      type="search"
      placeholder="Search Songs/Artists"
      :value="search"
      @input="searchSongs"
    />
    <div class="flex-grow-1 my-2" :style="{ overflowY: 'auto' }">{searchResults.map(track => {
      <TrackSearchResult track={track} key={track.uri} />
      })}</div>
    <div>Bottom</div>
  </div>
</template>

<script>
import axios from 'axios';
import SpotifyWebApi from 'spotify-web-api-node';
import TrackSearchResult from '../components/TrackSearchResult'
const spotifyApi = new SpotifyWebApi({
  clientId: '58c63d6c452940378c16d1e4a5a65ee0',
});

export default {
  name: 'Dashboard',
  props: {
    code: String,
  },
  components: {
    TrackSearchResult,
  },
  data() {
    return {
      accessToken: '',
      refreshToken: '',
      expiresIn: '',
      dashCode: this.code,
      search: '',
      searchResults: [],
    };
  },
  watch: {
    refreshToken: function() {
      this.refresh();
    },
    accessToken: function() {
      this.access();
      this.updateSearchResults();
    },
    search: function() {
      this.updateSearchResults();
    },
  },
  methods: {
    async login() {
      const code = this.dashCode;
      try {
        const res = await axios.post('http://localhost:3001/login', {
          code,
        });
        this.accessToken = res.data.accessToken;
        this.refreshToken = res.data.refreshToken;
        this.expiresIn = res.data.expiresIn;
        window.history.pushState({}, null, '/');
        return res.data.accessToken;
      } catch {
        window.location = '/';
      }
    },
    async refresh() {
      const refreshToken = this.refreshToken;
      try {
        const res = await axios.post('http://localhost:3001/refresh', {
          refreshToken,
        });
        this.accessToken = res.data.accessToken;
        this.expiresIn = res.data.expiresIn;
        return res.data.accessToken;
      } catch {
        window.location = '/';
      }
    },
    searchSongs(e) {
      this.search = e.target.value;
    },
    access() {
      if (!this.accessToken) return;
      spotifyApi.setAccessToken(this.accessToken);
    },
    async updateSearchResults() {
      if (!this.search) return (this.searchResults = []);
      spotifyApi.searchTracks(this.search).then(res => {
        this.searchResults = res.body.tracks.items.map(track => {
          const smallestAlbumImage = track.album.images.reduce(
            (smallest, image) => {
              if (image.height < smallest.height) return image;
              return smallest;
            },
            track.album.images[0]
          );
          return {
            artist: track.artists[0].name,
            title: track.name,
            uri: track.uri,
            albumUrl: smallestAlbumImage.url,
          };
        });
      });
      console.log("searchResults", this.searchResults);
      return this.searchResults;
    },
  },
  created() {
    this.login();
  },
};
</script>

<style scoped></style>
