<script setup lang="ts">
import { ref, computed } from "vue";
import { Button } from "@/components/ui/button";
import { GitBranch, Moon, Sun, Braces } from "lucide-vue-next";
import { useColorMode } from "@vueuse/core";

defineProps({
  appName: String
});

const mode = useColorMode({ initialMode: "system" });
const isDark = computed(() => mode.value === "dark");

const toggleMode = () => {
  mode.value = isDark.value ? "light" : "dark";
};
</script>

<template>
  <header
    class="bg-card sticky top-0 z-50 flex items-center justify-between border-b px-6 py-4 shadow-md h-20"
  >
    <div class="my-2 flex items-center space-x-2 hover:text-primary">
      <a href="#" class="flex items-center space-x-2">
        <Braces class="h-8 w-8" />
        <h1 class="text-xl font-bold">{{ appName }}</h1>
      </a>
    </div>

    <div class="flex items-center gap-2">
      <Button @click="toggleMode" variant="outline" size="icon" class="hover:text-primary">
        <Sun v-if="!isDark" />
        <Moon v-else />
        <span class="sr-only">Toggle theme</span>
      </Button>

      <Button variant="outline" size="icon" as-child class="hover:text-primary">
        <a href="https://github.com/Julestblt/JSON-parser" target="_blank">
          <GitBranch class="h-4 w-4" />
        </a>
      </Button>
    </div>
  </header>
</template>
