<script setup lang="ts">
import { ref, defineProps } from "vue";
import { Button } from "@/components/ui/button";
import { WandSparkles, ListTree, CircleX } from "lucide-vue-next";
import { JsonBeautify, JsonTree } from "@/components/ui/json";

const props = defineProps<{ json: string; isJsonValid: boolean }>();

const viewMode = ref<"beautify" | "tree">("beautify");

const setViewMode = (mode: "beautify" | "tree") => {
  viewMode.value = mode;
};
</script>

<template>
  <section class="relative h-full border rounded-lg p-4">
    <div class="flex justify-between items-center">
      <h3 class="text-xl font-bold">JSON Viewer</h3>
      <span class="flex items-center gap-4">
        <Button
          variant="secondary"
          @click="setViewMode('beautify')"
          :class="{ 'bg-blue-500 text-white': viewMode === 'beautify' }"
        >
          <WandSparkles class="w-4 h-4" />
          Beautify
        </Button>
        <Button
          variant="secondary"
          @click="setViewMode('tree')"
          :class="{ 'bg-blue-500 text-white': viewMode === 'tree' }"
        >
          <ListTree class="w-4 h-4" />
          TreeView
        </Button>
      </span>
    </div>

    <p
      v-if="json === ''"
      class="absolute top-[5rem] left-4 text-sm border p-4 rounded flex items-center gap-2 text-orange-500"
    >
      <CircleX class="w-4 h-4" />
      Enter a JSON to preview it.
    </p>

    <p
      v-else-if="!isJsonValid"
      class="absolute top-[5rem] left-4 text-sm border p-4 rounded flex items-center gap-2 text-red-500"
    >
      <CircleX class="h-4 w-4" />
      Invalid JSON
    </p>

    <div v-else class="absolute left-0 right-0 top-[5rem] bottom-0 overflow-auto">
      <JsonBeautify v-if="viewMode === 'beautify'" :json="json" class="h-full" />
      <JsonTree v-else :json="json" class="h-full" />
    </div>
  </section>
</template>
