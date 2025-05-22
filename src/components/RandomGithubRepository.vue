<script lang="ts" setup>
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card";
import LanguageSelector from "@/components/LanguageSelector.vue";
import { ref, computed } from "vue";
import GithubRepositoryCardLoader from "@/components/GithubRepositoryCardLoader.vue";
import { Star, GitFork, AlertCircle, LoaderCircle } from "lucide-vue-next";
import { Button } from "@/components/ui/button";
import GithubRepositoryCardError from "@/components/GithubRepositoryCardError.vue";
import ThemeSwitch from "@/components/ThemeSwitch.vue";
interface GithubRepository {
  id: number;
  name: string;
  owner: {
    login: string;
    avatar_url: string;
    html_url: string;
  };
  description: string;
  stargazers_count: number;
  html_url: string;
  forks_count: number;
  open_issues_count: number;
}

const selectedLanguage = ref<string>();
const isLoading = ref(false);
const reposToFetch = 5;
const randomRepositories = ref<GithubRepository[]>([]);
const wasLoaded = ref(false);
const apiCallError = ref<Error>();
const reposExists = computed(() => {
  return randomRepositories.value.length > 0;
});

async function selectLanguage(language: string) {
  wasLoaded.value = false;
  selectedLanguage.value = language;
  await fetchRandomRepositories(language);
}

async function fetchRandomRepositories(language: string) {
  isLoading.value = true;
  apiCallError.value = undefined;
  try {
    const response = await fetch(
      `https://api.github.com/search/repositories?q=language:${language}&sort=stars&order=desc`
    );
    const data = await response.json();
    const randomRepos: GithubRepository[] = [];
    for (let i = 0; i < reposToFetch; i++) {
      const randomIndex = Math.floor(Math.random() * 101);
      if (data.items[randomIndex]) {
        randomRepos.push(data.items[randomIndex]);
      }
    }
    randomRepositories.value = randomRepos;
  } catch (error) {
    randomRepositories.value = [];
    apiCallError.value = error as Error;
  } finally {
    isLoading.value = false;
    wasLoaded.value = true;
  }
}
</script>
<template>
  <Card>
    <CardHeader class="flex justify-between items-center">
      <div class="flex gap-2 flex-col">
        <CardTitle>Random Github Repository Project</CardTitle>
        <CardDescription>
          There you can select type of language and get random repository from
          Github.
        </CardDescription>
      </div>
      <div class="w-fit"><ThemeSwitch /></div>
    </CardHeader>
    <CardContent class="flex flex-col gap-4">
      <LanguageSelector @languageSelected="selectLanguage" />
      <Card>
        <CardHeader>
          <CardTitle>Random Repositories</CardTitle>
          <CardDescription>
            {{
              !!reposExists
                ? `Showing ${randomRepositories.length} random repositories`
                : "No repositories found"
            }}
          </CardDescription>
          <Button
            @click="async () => await fetchRandomRepositories(selectedLanguage)"
            class="w-fit mt-2"
            variant="secondary"
            :disabled="isLoading"
            v-if="wasLoaded"
          >
            Re-fetch
            <span class="animate-spin" v-if="isLoading"><LoaderCircle /></span
          ></Button>
        </CardHeader>
        <CardContent>
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
            <template v-if="!!apiCallError">
              <GithubRepositoryCardError :error="apiCallError" />
            </template>
            <template v-if="isLoading">
              <GithubRepositoryCardLoader v-for="i in reposToFetch" :key="i" />
            </template>
            <template v-else-if="!!reposExists">
              <Card
                v-for="repository in randomRepositories"
                :key="repository.id"
                class="w-full flex flex-col justify-between"
              >
                <CardHeader>
                  <CardTitle>
                    <a
                      :href="repository.html_url"
                      target="_blank"
                      rel="noopener noreferrer"
                      class="text-blue-500 hover:underline"
                    >
                      {{ repository.name }}
                    </a>
                  </CardTitle>
                  <CardDescription>
                    {{ repository.description }}
                  </CardDescription>
                </CardHeader>
                <CardFooter
                  class="flex flex-col justify-between items-start gap-2"
                >
                  <a
                    class="flex items-center gap-2"
                    :href="repository.owner.html_url"
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    <img
                      :src="repository.owner.avatar_url"
                      alt="Owner Avatar"
                      class="w-8 h-8 rounded-full"
                    />
                    <p class="">{{ repository.owner.login }}</p>
                  </a>
                  <div class="flex gap-4 text-sm text-muted-foreground">
                    <div class="flex items-center">
                      <Star class="size-4 mr-1" />
                      {{ repository.stargazers_count }}
                    </div>
                    <div class="flex items-center">
                      <GitFork class="size-4 mr-1" />
                      {{ repository.forks_count }}
                    </div>
                    <div class="flex items-center">
                      <AlertCircle class="size-4 mr-1" />
                      {{ repository.open_issues_count }}
                    </div>
                  </div>
                </CardFooter>
              </Card>
            </template>
          </div>
        </CardContent>
      </Card>
    </CardContent>
  </Card>
</template>
