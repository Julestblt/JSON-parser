<script setup lang="ts">
import { computed, defineProps } from "vue";
import { Button } from "@/components/ui/button";
import { Copy } from "lucide-vue-next";
import { toast } from "vue-sonner";

const props = defineProps<{ json: string }>();

const formattedJson = computed(() => {
  try {
    return JSON.stringify(JSON.parse(props.json), null, 2);
  } catch {
    return props.json;
  }
});

const jsonLines = computed(() =>
  formattedJson.value.split("\n").map((line) => ({
    number: true,
    content: line
  }))
);

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
  <div class="relative font-mono px-4 pb-4">
    <div class="relative border rounded h-full overflow-y-scroll pl-1">
      <Button
        variant="outline"
        class="absolute top-2 right-2 p-2 flex items-center gap-2"
        @click="copyToClipboard"
      >
        <Copy class="w-4 h-4" />
      </Button>

      <div class="flex flex-col">
        <div v-for="(line, index) in jsonLines" :key="index" class="flex items-start">
          <span class="w-10 text-right select-none text-gray-500 pr-4 border-r mr-2">{{
            index + 1
          }}</span>
          <span class="whitespace-pre-wrap break-words flex-1">{{ line.content }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
