<script setup lang="ts">
import { computed, defineProps, defineEmits } from "vue";
import { Button } from "@/components/ui/button";
import { Textarea } from "@/components/ui/textarea";
import { ClipboardPaste, Eraser, CircleX } from "lucide-vue-next";

const props = defineProps<{ json: string; isJsonValid: boolean }>();
const emit = defineEmits(["update:json"]);

const jsonValue = computed({
  get: () => props.json,
  set: (value) => emit("update:json", value)
});

const paste = async () => {
  const text = await navigator.clipboard.readText();
  emit("update:json", text);
};

const clear = () => emit("update:json", "");
</script>

<template>
  <section class="relative h-full border rounded-lg p-4">
    <div class="flex justify-between items-center mb-4">
      <h3 class="text-xl font-bold">JSON Entry</h3>
      <span class="flex items-center gap-4">
        <Button variant="secondary" @click="paste">
          <ClipboardPaste class="w-4 h-4" />
          Paste
        </Button>
        <Button variant="secondary" @click="clear">
          <Eraser class="w-4 h-4" />
          Clear
        </Button>
      </span>
    </div>

    <div class="absolute left-0 right-0 top-[4rem] bottom-0 overflow-auto p-4 pb-[2.25rem]">
      <Textarea
        v-model="jsonValue"
        class="w-full h-full resize-none"
        placeholder="JSON code here..."
      />
    </div>

    <p
      v-if="!isJsonValid && props.json !== ''"
      class="absolute bottom-2 left-4 text-red-500 text-sm flex items-center gap-2"
    >
      <CircleX class="h-4 w-4" />
      Invalid JSON
    </p>
  </section>
</template>
