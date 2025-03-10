<script setup lang="ts">
import { defineProps } from "vue";

const props = defineProps<{ json: string }>();
</script>

<template>
  <div class="relative text-white font-mono px-4 pb-4">
    <div class="relative border rounded h-full bg-gray-900 overflow-y-scroll pl-1">
      <pre>{{ JSON.parse(json) }}</pre>
    </div>
  </div>
</template>
