<template>
    <div class="flex flex-col w-[50rem] mx-auto mt-40 justify-center items-center gap-4">
        <input @change="changeTimeAudio" value="0" ref="seek" :max="audioDuration" type="range"
            class="-translate-y-4 w-[60rem]" step="1" />
        <audio @timeupdate="changeSeek" ref="audio" preload="metadata"></audio>
        <div class="flex gap-12">
            <button @click="activeSongIndex = activeSongIndex > 0 ? (activeSongIndex - 1) % songList.length : songList.length - 1">
                <!-- BACKWARD ICON -->
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                    stroke="currentColor" class="w-10 h-10 text-sky-900">
                    <path stroke-linecap="round" stroke-linejoin="round"
                        d="M21 16.811c0 .864-.933 1.405-1.683.977l-7.108-4.062a1.125 1.125 0 010-1.953l7.108-4.062A1.125 1.125 0 0121 8.688v8.123zM11.25 16.811c0 .864-.933 1.405-1.683.977l-7.108-4.062a1.125 1.125 0 010-1.953L9.567 7.71a1.125 1.125 0 011.683.977v8.123z" />
                </svg>
            
            </button>
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
            <button @click="activeSongIndex = (activeSongIndex + 1) % songList.length">
                <!-- FORWARD ICON  -->
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                    stroke="currentColor"  class="w-10 h-10 text-sky-900">
                    <path stroke-linecap="round" stroke-linejoin="round"
                        d="M3 8.688c0-.864.933-1.405 1.683-.977l7.108 4.062a1.125 1.125 0 010 1.953l-7.108 4.062A1.125 1.125 0 013 16.81V8.688zM12.75 8.688c0-.864.933-1.405 1.683-.977l7.108 4.062a1.125 1.125 0 010 1.953l-7.108 4.062a1.125 1.125 0 01-1.683-.977V8.688z" />
                </svg>
            
            </button>
        </div>

        <input value="100" class="translate-y-4 w-36" @change="changeVolume" type="range" />
        <SongsList :activeSongIndex="activeSongIndex" @set-song="(index) => { activeSongIndex = index }"></SongsList>
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
const activeSongIndex = ref(0);

onMounted(() => {
    audio.value.src = '../assets/mp3/' + songList[0];
})

watch(activeSongIndex, () => {
    setSong(activeSongIndex.value);
})

const changeVolume = (e) => {
    audio.value.volume = e.currentTarget.value / 100;
}

const changeSeek = () => {
    seek.value.value = Math.floor(audio.value.currentTime)
    if (audio.value.currentTime == audio.value.duration) {
        activeSongIndex.value = (activeSongIndex.value + 1) % songList.length;
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
    console.log(activeSongIndex.value)
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