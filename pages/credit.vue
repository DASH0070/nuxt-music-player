<template>
    <div class="flex justify-center gap-10 flex-col items-center">
        <h1 class="text-center text-3xl my-10">Music provided by NoCopyrightSounds.</h1>
        <div v-for="item in songList" :key="item.link">
            <p>{{ item.track }}</p>
            <a target="_blank" :href="item.link" class="text-blue-400">{{ item.link }}</a>
        </div>
        <NuxtLink class="text-slate-50 bg-sky-900 rounded-full px-4 py-2" to="/">Go Back</NuxtLink>
    </div>
</template>

<script setup lang="ts">
import { songList } from '../data/songList';
</script>