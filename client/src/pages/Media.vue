<template>
  <div :class="{kidsTheme: isKid}">
    <logo-full></logo-full>
    <div v-if="types" class="types">
      <router-link
        v-for="type in types"
        :key="type.id"
        :to="'/media/' + type.value"
      >
        <button>
          <img :src="publicPath + type.icon" :alt="type.title" />
          <p>{{ type.title }}</p>
        </button>
      </router-link>
    </div>
  </div>
</template>

<script>
import fetchServer from "../util/requestsJwt";
import LogoFull from "../components/LogoFull.vue";

export default {
  components: {
    LogoFull,
  },
  data() {
    return {
      publicPath: process.env.BASE_URL,
      types: null,
      isKid: this.$store.getters.isKid
    };
  },
  mounted() {
    this.getTypes();
  },
  methods: {
    async getTypes() {
      try {
        const response = await fetchServer(
          "/api/media/types",
          "GET",
          this.$store.getters.token
        );
        this.types = response.types;
      } catch (err) {
        await this.$store.dispatch("setError", { error: err });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../assets/scss/_variables.scss";

.types {
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
  width: 100%;
  padding: 5% 10%;

  button {
    background-color: transparent;
    outline: none;
    border: none;
    color: $color-text;
    font-size: 2.4rem;
    transition: background-color 0.2s;
    padding: 1.5rem 2.5rem;
    margin: 1rem 0;
    border-radius: 1rem;

    &:hover {
      background-color: $color-dark-overlay;
    }
  }
}

</style>