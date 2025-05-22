<script setup lang="ts">
import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import { useColorMode } from "@vueuse/core";
import { SunMoon } from "lucide-vue-next";

const mode = useColorMode();
</script>
<template>
  <DropdownMenu>
    <DropdownMenuTrigger as-child>
      <Button variant="secondary">
        <SunMoon />
        <span>Toggle theme</span>
      </Button>
    </DropdownMenuTrigger>
    <DropdownMenuContent align="end">
      <DropdownMenuItem
        @click="mode = 'light'"
        :class="{ 'bg-muted font-semibold': mode === 'light' }"
      >
        Light
      </DropdownMenuItem>
      <DropdownMenuItem
        @click="mode = 'dark'"
        :class="{ 'bg-muted font-semibold': mode === 'dark' }"
      >
        Dark
      </DropdownMenuItem>
      <DropdownMenuItem
        @click="mode = 'auto'"
        :class="{ 'bg-muted font-semibold': mode === 'auto' }"
      >
        System
      </DropdownMenuItem>
    </DropdownMenuContent>
  </DropdownMenu>
</template>
