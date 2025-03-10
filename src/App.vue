<script setup lang="ts">
import { Header, Main } from "./components/layout";
import { Toaster } from "@/components/ui/sonner";
</script>

<template>
  <Toaster />
  <Header appName="JSON parser" />
  <Main />
</template>
