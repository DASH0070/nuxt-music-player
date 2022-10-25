<template>
    <div class="w-full px-4 py-16">
        <div class="mx-auto w-full max-w-md">
            <RadioGroup v-model="selected">
                <RadioGroupLabel class="sr-only">Server size</RadioGroupLabel>
                <div class="space-y-2">
                    <RadioGroupOption @click="setSong(index)" as="template" v-for="(item, index) in songList"
                        :key="index" :value="item" v-slot="{ active, checked }">
                        <div :class="[
                            active
                                ? 'ring-2 ring-white ring-opacity-60 ring-offset-2 ring-offset-sky-300'
                                : '',
                            checked ? 'bg-sky-900 bg-opacity-75 text-white ' : 'bg-white ',
                        ]" class="relative flex cursor-pointer rounded-lg px-5 py-4 shadow-md focus:outline-none">
                            <div class="flex w-full items-center justify-between">
                                <div class="flex items-center">
                                    <div class="text-sm">
                                        <RadioGroupLabel as="p" :class="checked ? 'text-white' : 'text-gray-900'"
                                            class="font-medium">
                                            {{ item.track }}
                                        </RadioGroupLabel>
                                    </div>
                                </div>
                                <div v-show="checked" class="shrink-0 text-white">
                                    <svg class="h-6 w-6" viewBox="0 0 24 24" fill="none">
                                        <circle cx="12" cy="12" r="12" fill="#fff" fill-opacity="0.2" />
                                        <path d="M7 13l3 3 7-7" stroke="#fff" stroke-width="1.5" stroke-linecap="round"
                                            stroke-linejoin="round" />
                                    </svg>
                                </div>
                            </div>
                        </div>
                    </RadioGroupOption>
                </div>
            </RadioGroup>
        </div>
    </div>
</template>
  
<script setup lang="ts">
import { ref } from 'vue'
import {
    RadioGroup,
    RadioGroupLabel,
    RadioGroupOption,
} from '@headlessui/vue';
import { songList } from '../data/songList';

const props = defineProps<{ activeSongIndex }>();   // which song is in active state
const emit = defineEmits(['setSong'])

const setSong = (index) => {    // change the song active state
    emit('setSong', index)
}
const selected = ref(songList[props.activeSongIndex]);      // which box is currently selected
const selectedValue = computed(() => songList[props.activeSongIndex]);      // which song is active
watch(selectedValue, () => selected.value = selectedValue.value)        // set currently selected box if song switch
</script>
