<template lang='pug'>
nav
  input(v-model="name" placeholder="Имя")
  select(v-model="status" )
    option(v-for="s in statusList") {{ s }}
  button(@click="filter") Применить
</template>

<script>
export default {
  data: () => ({
    name: "",
    status: "",
    statusList: ["unknown", "Alive", "Dead"],
  }),
  methods: {
    filter() {
      this.$emit("filter", { name: this.name, status: this.status });
    },
  },
  emits: {
    filter: null,
  },
};
</script>

<style scoped>
nav {
  @apply grid grid-cols-8 gap-4 text-[#333333];
}
input {
  @apply border-2 border-[#ff9900] col-span-6 rounded-[0.5rem] pl-[1rem];
}
select {
  @apply rounded-[0.5rem] text-center;
}
button {
  @apply border-2 border-[#ff9900] bg-[#3c3e44] rounded-[0.5rem] font-bold text-[#f5f5f5];
}
</style>