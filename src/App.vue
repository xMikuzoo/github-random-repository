<script lang="ts" setup>
import { useDark, useToggle } from "@vueuse/core";
import { Button } from "@/components/ui/button";
import { SunMoon, Github } from "lucide-vue-next";
import RandomGithubRepository from "@/components/RandomGithubRepository.vue";

const isDark = useDark();
const toggleDark = useToggle(isDark);
</script>
<template>
  <div class="min-h-[100dvh] flex flex-col justify-between">
    <header class="bg-secondary p-2">
      <Button @click="toggleDark()"><SunMoon />Toggle theme</Button>
    </header>
    <main>
      <RandomGithubRepository />
    </main>
    <footer
      class="bg-secondary p-2 min-h-[10dvh] flex items-center justify-center"
    >
      <p class="w-fit text-center">
        Created by
        <a
          class="font-semibold flex gap-2 items-center hover:border-b hover:border-b-accent-foreground"
          href="https://github.com/xMikuzoo"
          target="_blank"
        >
          <Github class="size-4" />xMikuzoo
        </a>
      </p>
    </footer>
  </div>
</template>
