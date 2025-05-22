<script lang="ts" setup>
import { useDark } from "@vueuse/core";
import RandomGithubRepository from "@/components/RandomGithubRepository.vue";

const isDark = useDark();
</script>
<template>
  <main class="min-h-[100dvh] p-2">
    <RandomGithubRepository />
  </main>
</template>
