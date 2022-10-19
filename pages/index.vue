<template>
    <div class="flex flex-col w-[50rem] mx-auto mt-40 justify-center items-center gap-4">
        <input @change="changeTimeAudio" value="0" ref="seek" :max="audioDuration" type="range" class="-translate-y-4 w-[60rem]" step="1" />
        <audio @timeupdate="changeSeek" ref="audio" preload="metadata"></audio>
        <button @click="music" class=" rounded-full bg-slate-200 p-1">
            <!-- PLAY ICON  -->
            <svg v-show="!isAudioPlay" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                stroke-width="1.5" stroke="currentColor" class="w-10 h-10 text-sky-900 translate-x-0.5">
                <path stroke-linecap="round" stroke-linejoin="round"
                    d="M5.25 5.653c0-.856.917-1.398 1.667-.986l11.54 6.348a1.125 1.125 0 010 1.971l-11.54 6.347a1.125 1.125 0 01-1.667-.985V5.653z" />
            </svg>
            <!-- PAUSE ICON -->
            <svg v-show="isAudioPlay" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                stroke-width="1.5" stroke="currentColor" class="w-10 h-10 text-sky-900">
                <path stroke-linecap="round" stroke-linejoin="round" d="M15.75 5.25v13.5m-7.5-13.5v13.5" />
            </svg>
        </button>

        <input value="100" class="translate-y-4 w-36" @change="changeVolume"
            type="range" />
        <SongsList :activeSongIndex="activeSongIndex"></SongsList>
    </div>
</template>

<script setup lang="ts">
import SongsList from "../components/SongsList.vue";
import { songList } from '../data/songList';

// ref declaration
const audio = ref();
const audioDuration = ref(0);   // for the duration of song
const isAudioPlay = ref(false)    // to check state of song play/pause
const seek = ref();
const activeSongIndex = ref({ index: 0 });

onMounted(() => {
    audio.value.src = '../assets/mp3/The Chemical Brothers - Galvanize.mp3';
})

watch(activeSongIndex.value, () => {
    setSong(activeSongIndex.value.index);
})

const changeVolume = (e) => {
    audio.value.volume = e.currentTarget.value / 100;
}

const changeSeek = () => {
    seek.value.value = Math.floor(audio.value.currentTime)
    if (audio.value.currentTime == audio.value.duration) {
        activeSongIndex.value.index = (activeSongIndex.value.index + 1) % 10;
    }
}

const changeTimeAudio = () => {
    audio.value.currentTime = seek.value.value
}

// Handle Pause/Play Button
const music = () => {
    if (!audio.value.src) {
        alert('Select Song');
        return;
    }
    isAudioPlay.value = !isAudioPlay.value
    if (isAudioPlay.value) {
        audioDuration.value = audio.value.duration;
        audio.value.play();
    }
    else if (!isAudioPlay.value) {
        audio.value.pause();
    }
}

// Change selected song
const setSong = (index: number) => {
    if (isAudioPlay.value) {
        audio.value.pause();
        audio.value.src = '../assets/mp3/' + songList[index];
        audio.value.play();
        audio.value.onloadedmetadata = function () {
            audioDuration.value = audio.value.duration;
        }
    }
    else
        audio.value.src = '../assets/mp3/' + songList[index];

}

</script>