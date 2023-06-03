<template>
  <label
    class="flex w-[calc(100%-80px)] cursor-pointer rounded-md border-[1px] border-inherit px-[12px] py-[10px] md:w-[600px]">
    <IconSearch />
    <input type="text" placeholder="Search" class="ml-[12px] w-[100%] outline-0" :value="search"
      @input="$emit('update:search', $event.target.value)" />
  </label>
</template>

<script setup>
import IconSearch from "./icons/IconSearch.vue";
defineProps({
  search: String,
});
defineEmits(["update:search"]);
</script>
