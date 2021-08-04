<template>
  <v-container class="home">
    <h1 class="red-shadow text-center">Owl Dictionary</h1>
    <form class="input-box" @submit.prevent="meaning">
      <input placeholder="search for a word..." v-model="word" />
      <v-btn
        color="#fe1292"
        tile
        height="60"
        width="60"
        type="submit"
        :loading="load"
      >
        <v-icon color="#fff">mdi-magnify</v-icon>
      </v-btn>
    </form>
    <p class="red-shadow text-center">
      made by
      <a class="link" target="_blank" href="https://github.com/mRiloB"
        >@mRiloB</a
      >
      with the
      <a class="link" target="_blank" href="https://owlbot.info/">Owlbot API</a>
    </p>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data: () => ({
    word: "",
    load: false,
  }),
  methods: {
    async meaning() {
      if (!this.word.trim()) return;
      this.load = true;
      try {
        const res = await this.search_word();
        this.$router.push({
          name: "meaning",
          params: { data: res },
        });
      } catch (err) {
        console.log(err);
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
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100%;
}

.link {
  color: #fff;
  text-decoration: none;
}
.link:active {
  color: #fff;
}
.link:hover {
  text-decoration: underline;
}

.input-box {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  input {
    border: 1px solid #fe1292;
    height: 60px;
    padding: 0 10px;
    outline: 0;
    width: 240px;
  }

  input,
  input::placeholder {
    font-weight: bold;
    color: #fff;
  }
}

.red-shadow {
  color: #fff;
  text-shadow: 0px 4px 4px #fe1292;
}
</style>
