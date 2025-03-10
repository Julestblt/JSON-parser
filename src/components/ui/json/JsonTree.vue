<script setup lang="ts">
import { defineProps, computed } from "vue";
import { JsonTreeNode } from "./";

const props = defineProps<{ json: string }>();

const parsedJson = computed(() => {
  try {
    return JSON.parse(props.json);
  } catch {
    return null;
  }
});
</script>

<template>
  <div class="relative font-mono px-4 pb-4">
    <div class="relative border rounded h-full overflow-y-auto p-2">
      <JsonTreeNode v-if="parsedJson" :node="parsedJson" :json="props.json" />
      <p v-else class="text-red-500">Invalid JSON</p>
    </div>
  </div>
</template>
