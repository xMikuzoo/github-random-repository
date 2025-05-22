<script lang="ts" setup>
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
</script>
<template>
  <Card class="w-full max-w-sm mx-auto mt-10">
    <CardHeader>
      <CardTitle>Random Github Repository</CardTitle>
      <CardDescription>
        This is a random Github repository from the API.
      </CardDescription>
    </CardHeader>
    <CardContent>
      <p class="text-center">Loading...</p>
    </CardContent>
</template>
