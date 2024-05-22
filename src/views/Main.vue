<template lang='pug'>
.main
  .nav-pag(v-if="!loading")
    .page(
      v-for="p in pages",
      @click="goToPage(p)",
      :class="{ underlined: p === page }"
    ) {{ p }}
  .article-list(v-if="!loading")
    article.article__wrapper(v-for="c in characters")
      img(alt="c.name", :src="c.image")
      .article__inner
        h1 {{ c.name }}
        .status
          .status-icon(:class="iconColor(c.status)")
          span {{ c.status }} - {{ c.species }}
        h3 Last known location:
        p {{ c.location.name }}
  .loader(v-else)
</template>

<script>
export default {
  props: {
    filterByName: String,
    filterByStatus: String,
  },
  data: () => ({
    characters: [],
    pages: 1,
    page: 1,
    loading: false,
  }),
  watch: {
    filterByName(val) {
      this.fetchCharacters();
    },
    filterByStatus(val) {
      this.fetchCharacters();
    },
  },
  methods: {
    goToPage(page) {
      this.page = page;
      this.fetchCharacters();
    },
    fetchPagesTotal() {
      //promise resolve
      return fetch(
        "https://rickandmortyapi.com/api/character/?" +
          (this.filterByName ? "name=" + this.filterByName : "") +
          (this.filterByStatus ? "&status=" + this.filterByStatus : "")
      )
        .then((res) => res.json())
        .then((json) => (this.pages = json?.info?.pages));
    },
    fetchCharacters() {
      this.loading = true;
      this.fetchPagesTotal().then(
        fetch(
          "https://rickandmortyapi.com/api/character/?page=" +
            +this.page +
            (this.filterByName ? "&name=" + this.filterByName : "") +
            (this.filterByStatus ? "&status=" + this.filterByStatus : "")
        )
          .then((res) => res.json())
          .then((json) => {
            this.characters = json.results;
            this.loading = false;
          })
      );
    },
    iconColor(status) {
      switch (status) {
        case "unknown":
          return "icon-gray";
        case "Alive":
          return "icon-green";
        case "Dead":
          return "icon-red";
      }
    },
  },
  mounted() {
    this.fetchCharacters();
  },
};
</script>

<style scoped>
.main {
  @apply flex flex-col items-center justify-center;
}
.article-list {
  @apply grid grid-cols-2;
}
.article__wrapper {
  @apply flex w-[600px] h-[220px] bg-[#3c3e44] m-[0.75rem] rounded-[0.5rem] overflow-hidden;
}
.article__inner {
  @apply p-[0.75rem];
}
h1 {
  @apply text-[1.5rem] font-[800];
}
h2 {
  @apply text-[16px];
}
h3 {
  @apply text-gray-400 mt-[8px] mb-[4px];
}
.status {
  @apply flex items-center;
}
.status-icon {
  @apply w-[0.5rem] h-[0.5rem] rounded-full mr-[4px];
}
.icon-gray {
  @apply bg-gray-400;
}
.icon-green {
  @apply bg-green-400;
}
.icon-red {
  @apply bg-red-400;
}
.nav-pag {
  @apply flex;
}
.page {
  @apply cursor-pointer m-[2px];
}
.underlined {
  @apply underline;
}
.loader {
  width: 48px;
  height: 48px;
  border: 5px solid #fff;
  border-bottom-color: transparent;
  border-radius: 50%;
  display: inline-block;
  box-sizing: border-box;
  margin-top: 80px;
  animation: rotation 1s linear infinite;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>