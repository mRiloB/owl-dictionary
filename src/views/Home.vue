<template>
  <v-container class="home">
    <h1 class="text-white-with-red-shadow">Owl Dictionary</h1>
    <div class="input-box">
      <input
        type="text"
        placeholder="Search for a word..."
        v-model="word"
        @keypress.enter="meaning"
      />
      <v-btn
        tile
        color="#FE1292"
        height="50"
        width="50"
        :loading="load"
        @click="meaning"
      >
        <v-icon color="#FFF">mdi-magnify</v-icon>
      </v-btn>
    </div>
    <footer>
      <p>
        made by
        <a :href="insta" target="_blank">Murilo B.</a>
        with the
        <a :href="owlbot" target="_blank">Owlbot API</a>
      </p>
    </footer>
  </v-container>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";

export default {
  name: "Home",
  data: () => ({
    word: "",
    load: false,
    insta: "https://www.instagram.com/mbeni.wav/",
    git: "https://github.com/mRiloB",
    owlbot: "https://owlbot.info/",
  }),
  methods: {
    async meaning() {
      const word = this.word.trim();
      if (!word) return;
      try {
        this.load = true;
        const res = await this.search_word();
        this.$router.push({ name: "meaning", params: { word_data: res } });
      } catch (err) {
        const message = err.response.data[0].message;
        const errMsg = err.message || err;
        Swal.fire({
          title: "Oops!",
          text: message || errMsg,
          icon: "warning",
          confirmButtonText: "Try again",
          confirmButtonColor: "#FE1292"
        });
      } finally {
        this.load = false;
      }
    },
    async search_word() {
      const res = await axios({
        url: "https://owlbot.info/api/v4/dictionary/" + this.word.trim(),
        method: "GET",
        headers: {
          Authorization: "Token " + process.env.VUE_APP_APIKEY,
        },
      });
      return res.data;
    },
  },
};
</script>

<style lang="scss" scoped>
.home {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: stretch;
}

h1,
input,
input::placeholder {
  color: #fff;
  font-weight: bold;
}

.text-white-with-red-shadow {
  text-shadow: 0 4px 1px #fe1292;
}

h1 {
  text-align: center;
}

.input-box {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  input {
    border: none;
    border-top: 2px solid #fe1292;
    border-bottom: 2px solid #fe1292;
    border-left: 2px solid #fe1292;
    height: 50.5px;
    width: 100%;
    padding: 0 5px;
    outline: 0;
  }
}

footer {
  p,
  a,
  a:active {
    color: rgba(255, 255, 255, 0.3);
    font-weight: bold;
    text-align: center;
  }

  a {
    text-decoration: none;
  }

  a:hover {
    text-decoration: underline;
  }
}
</style>
