<script setup lang="ts">
import { ref, onMounted } from "vue";
import { Button } from "@/components/ui/button";
import {
  Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
} from "@/components/ui/command";
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";

interface ProgrammingLanguage {
  title: string;
  label: string;
}

const emit = defineEmits(["languageSelected"]);

const open = ref(false);
const programmingLanguages: ProgrammingLanguage[] =
  ref<ProgrammingLanguage[]>();
const selectedProgrammingLangague = ref<ProgrammingLanguage>();

onMounted(async () => {
  await fetch(
    "https://raw.githubusercontent.com/kamranahmedse/githunt/master/src/components/filters/language-filter/languages.json"
  )
    .then((response) => response.json())
    .then((data) => {
      programmingLanguages.value = data;
    });
});
</script>

<template>
  <div class="flex items-center space-x-4">
    <p class="text-sm text-muted-foreground">Programming Language</p>
    <Popover v-model:open="open">
      <PopoverTrigger as-child>
        <Button
          variant="outline"
          size="sm"
          class="w-fit justify-start hover:cursor-pointer"
        >
          <template v-if="selectedProgrammingLangague">
            {{ selectedProgrammingLangague?.title }}
          </template>
          <template v-else>Select programming language </template>
        </Button>
      </PopoverTrigger>
      <PopoverContent class="p-0" side="bottom" align="start">
        <Command>
          <CommandInput placeholder="Change status..." />
          <CommandList>
            <CommandEmpty>No results found.</CommandEmpty>
            <CommandGroup>
              <CommandItem
                v-for="programmingLangage in programmingLanguages"
                :key="programmingLangage.value"
                :value="programmingLangage.value"
                @select="
                  () => {
                    selectedProgrammingLangague = programmingLangage;
                    open = false;
                    emit('languageSelected', programmingLangage);
                  }
                "
              >
                {{ programmingLangage.title }}
              </CommandItem>
            </CommandGroup>
          </CommandList>
        </Command>
      </PopoverContent>
    </Popover>
  </div>
</template>
