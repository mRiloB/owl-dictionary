<template>
  <v-container class="about">
    <v-btn color="#fe1292" tile height="50" width="50" @click="back">
      <v-icon color="#fff">mdi-arrow-left</v-icon>
    </v-btn>
    <h1 class="text-white-with-red-shadow">{{ word_data.word }}</h1>
    <p class="pronuciation text-grey-italic">{{ word_data.pronunciation }}</p>
    <div
      class="definition"
      v-for="(def, id) in word_data.definitions"
      :key="id"
    >
      <p class="text-grey-italic">{{ def.type }}</p>
      <p>{{ def.definition }} {{ def.emoji || "" }}</p>
      <p class="text-grey-italic">example</p>
      <p>
        {{ def.example }}
      </p>
    </div>
  </v-container>
</template>

<script>
export default {
  name: "Meaning",
  data: () => ({
    word_data: {},
  }),
  mounted() {
    this.wordDataCheck();
  },
  methods: {
    wordDataCheck() {
      const word_data = this.$route.params.word_data;
      if (!word_data) this.$router.replace({ name: "home" });
      this.word_data = word_data;
    },
    back() {
      this.$router.go(-1);
    }
  },
};
</script>

<style lang="scss" scoped>
.text-grey-italic {
  color: #999;
}

.pronuciation {
  margin-left: 3px;
  font-size: 24px;
}

.about {
  h1 {
    font-size: 40px;
    margin: 20px 0 10px;
  }

  .definition {
    border-radius: 10px;
    padding: 10px;
    box-shadow: 4px 4px 1px #fe1292;
    background: #fff;
    margin-bottom: 15px;
    font-weight: 500;
  }
}
</style>
