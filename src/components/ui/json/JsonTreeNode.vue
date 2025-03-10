<script setup lang="ts">
import { defineProps, ref, computed } from "vue";
import { ChevronRight, Brackets, Copy } from "lucide-vue-next";
import { Button } from "@/components/ui/button";
import { toast } from "vue-sonner";

const props = defineProps<{ node: any; level?: number; json: string }>();

const formattedJson = computed(() => {
  try {
    return JSON.stringify(JSON.parse(props.json), null, 2);
  } catch {
    return props.json;
  }
});

const isOpen = ref(false);

const isObject = (val: any) => typeof val === "object" && val !== null;
const isArray = (val: any) => Array.isArray(val);
const isPrimitive = (val: any) =>
  ["string", "number", "boolean"].includes(typeof val) || val === null;

const copyToClipboard = async () => {
  try {
    await navigator.clipboard.writeText(formattedJson.value);
    toast("JSON copied !", {
      description: "Your formatted JSON has been copied to the clipboard."
    });
  } catch (err) {
    console.error("Failed to copy JSON:", err);
    toast("Copy failed", {
      description: "An error occurred while copying the JSON."
    });
  }
};
</script>

<template>
  <div class="ml-4">
    <Button
      variant="outline"
      class="absolute top-2 right-2 p-2 flex items-center gap-2 text-white"
      @click="copyToClipboard"
    >
      <Copy class="w-4 h-4" />
    </Button>
    <template v-if="isObject(node)">
      <div class="flex items-center space-x-2">
        <button @click="isOpen = !isOpen" class="text-gray-400 hover:text-primary">
          <ChevronRight
            class="w-4 h-4 transition-transform duration-200"
            :class="{ 'rotate-90': isOpen }"
          />
        </button>
        <Brackets class="w-4 h-4 text-gray-500" />
        <span class="text-gray-500">{ {{ isArray(node) ? "Array" : "Object" }} }</span>
      </div>
      <div v-if="isOpen" class="ml-4 border-l border-gray-700 pl-4">
        <div v-for="(value, key) in node" :key="key" class="mt-1">
          <template v-if="isPrimitive(value)">
            <div class="flex items-center gap-2">
              <span class="text-blue-400">"{{ key }}"</span>:
              <span :class="value === null ? 'text-gray-400' : 'text-green-400'">
                {{ value === null ? "null" : JSON.stringify(value) }}
              </span>
            </div>
          </template>
          <template v-else>
            <span class="text-blue-400">"{{ key }}"</span>:
            <JsonTreeNode :node="value" :level="(level ?? 0) + 1" :json="props.json" />
          </template>
        </div>
      </div>
    </template>

    <template v-else>
      <span class="text-green-400">{{ JSON.stringify(node) }}</span>
    </template>
  </div>
</template>
