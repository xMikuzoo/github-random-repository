<script lang="ts" setup>
const props = defineProps({
  error: {
    type: Error,
    required: true,
  },
});
</script>
<template>
  <div
    class="w-full border bg-destructive text-destructive-foreground shadow-sm rounded-lg p-6"
  >
    <div class="space-y-1">
      <h3 class="text-lg font-semibold">Error</h3>
      <p class="text-sm">
        {{ props.error.message }}
      </p>
    </div>
  </div>
</template>
