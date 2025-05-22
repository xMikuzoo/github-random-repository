<script lang="ts" setup>
import { useDark, useToggle } from "@vueuse/core";
import { Button } from "@/components/ui/button";
import { SunMoon, Github } from "lucide-vue-next";
import RandomGithubRepository from "@/components/RandomGithubRepository.vue";

const isDark = useDark();
const toggleDark = useToggle(isDark);
</script>
<template>
  <main class="min-h-[100dvh] p-2">
    <RandomGithubRepository />
  </main>
</template>
