<script setup>
import { ref, watch, provide } from "vue";
import SearchBox from "./components/SearchBox.vue";

const searchText = ref();
provide("searchText", searchText);
</script>

<template>
  <header
    class="flex items-center border-b-[1px] border-inherit px-[10px] py-[8px] pl-[30] md:px-[30px] md:pl-[80px]"
  >
    <span class="mr-[20px] text-[22px] text-[#5f6368] md:mr-[50px]">Todo</span>
    <SearchBox v-model:search="searchText" />
    <div class="float-right"></div>
    <!-- <nav>
      k
      <RouterLink to="/">Home</RouterLink>
      <RouterLink to="/about">About</RouterLink>
    </nav> -->
  </header>

  <RouterView />
</template>

<style scoped></style>
