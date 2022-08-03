<template>
  <div>
    <Pagination :totalPages="10" :perPage="10" />
  </div>
</template>

<script setup >
import "./index.css";
import Pagination from "./components/Pagination.vue";
</script>

<style>
#app {
  text-align: center;

  margin-top: 60px;
}
</style>
