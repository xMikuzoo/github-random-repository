<script lang="ts" setup>
import { Skeleton } from "@/components/ui/skeleton";
</script>
<template>
  <div
    class="w-full border bg-card text-card-foreground shadow-sm rounded-lg p-6"
  >
    <div class="space-y-2 mb-4">
      <Skeleton class="h-6 w-3/4" />
      <Skeleton class="h-4 w-full" />
    </div>
    <div class="space-y-2 mb-4">
      <Skeleton class="h-4 w-1/4" />
      <Skeleton class="h-4 w-1/4" />
      <Skeleton class="h-4 w-1/4" />
    </div>
    <div>
      <Skeleton class="h-4 w-1/3" />
    </div>
  </div>
</template>
