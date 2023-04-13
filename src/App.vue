<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <header>
      <nav>
        <RouterLink to="/">Calculator</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
  </header>

  <RouterView />
</template>

<style scoped>
header{
  text-align: center;
  margin-bottom: 30px;
}
header nav{
  display: flex;
  justify-content: center;
  gap: 20px;
}
</style>
