<template>
  <main class="main">
    <button class="add-button" @click="amount++">Add stopwatcher</button>
    <stopwatcher v-for="el in amount" :key="el" />
  </main>
</template>

<script setup>
import stopwatcher from "./components/stopwatcher.vue";
import { ref } from "vue";

const amount = ref(0);
</script>

<style lang="scss" scoped>
@import "./assets/mixins/button.scss";
.main {
  padding: 15px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.add-button {
  @include button(#2e2e2e);
  margin-bottom: 15px;
  background: #fff;
}
</style>
