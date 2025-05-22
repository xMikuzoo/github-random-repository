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
import { ref } from "vue";
import GithubRepositoryCardLoader from "@/components/GithubRepositoryCardLoader.vue";

interface GithubRepository {
  name: string;
  owner: {
    login: string;
    avatar_url: string;
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

async function selectLanguage(language: string) {
  selectedLanguage.value = language;
  await fetchRandomRepositories(language);
}

async function fetchRandomRepositories(language: string) {
  isLoading.value = true;
  const response = await fetch(
    `https://api.github.com/search/repositories?q=language:${language}&sort=stars&order=desc`
  );
  const data = await response.json();
  const randomRepos: GithubRepository[] = [];
  for (let i = 0; i < reposToFetch; i++) {
    const randomIndex = Math.floor(Math.random() * data.items.length);
    randomRepos.push(data.items[randomIndex]);
  }
  randomRepositories.value = randomRepos;
  isLoading.value = false;
}
</script>
<template>
  <Card>
    <CardHeader>
      <CardTitle>Random Github Repository Project</CardTitle>
      <CardDescription>
        There you can select type of language and get random repository from
        Github.
      </CardDescription>
    </CardHeader>
    <CardContent class="flex flex-col gap-4">
      <LanguageSelector @languageSelected="selectLanguage" />
      <Card>
        <CardHeader>
          <CardTitle>Random Repositories</CardTitle>
          <CardDescription>
            Here are some random repositories from Github.
          </CardDescription>
        </CardHeader>
        <CardContent>
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
            <template v-if="isLoading">
              <GithubRepositoryCardLoader v-for="i in reposToFetch" :key="i" />
            </template>
            <template v-else-if="randomRepositories.length > 0">
              <Card
                v-for="repository in randomRepositories"
                :key="repository.id"
                class="w-full"
              >
                <CardHeader>
                  <CardTitle>{{ repository.name }}</CardTitle>
                  <CardDescription>
                    {{ repository.description }}
                  </CardDescription>
                </CardHeader>
                <CardContent>
                  <p>Stars: {{ repository.stargazers_count }}</p>
                  <p>Forks: {{ repository.forks_count }}</p>
                  <p>Open Issues: {{ repository.open_issues_count }}</p>
                </CardContent>
                <CardFooter>
                  <a
                    :href="repository.html_url"
                    target="_blank"
                    class="text-blue-500 hover:underline"
                    >View on Github</a
                  >
                </CardFooter>
              </Card>
            </template>
          </div>
        </CardContent>
      </Card>
    </CardContent>
  </Card>
</template>
