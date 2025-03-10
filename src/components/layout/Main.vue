<script setup lang="ts">
import { ref, computed } from "vue";
import { JsonInput, JsonViewer } from "./";

const json = ref<string>("");

const isJsonValid = computed(() => {
  try {
    JSON.parse(json.value);
    return true;
  } catch (e) {
    return false;
  }
});
</script>

<template>
  <main
    class="h-[calc(100vh-5rem)] max-h-[calc(100vh-5rem)] mx-auto px-4 py-6 sm:px-6 lg:px-8 grid grid-cols-1 lg:grid-cols-2 gap-10"
  >
    <JsonInput v-model:json="json" :isJsonValid="isJsonValid" />
    <JsonViewer :json="json" :isJsonValid="isJsonValid" />
  </main>
</template>
