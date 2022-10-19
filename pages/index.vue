<template>
    <div class="flex flex-col w-[50rem] mx-auto mt-40 justify-center items-center gap-4">
        <audio @timeupdate="changeSeek" ref="audio"></audio>
        <button @click="music" class="w-20 h-8 bg-slate-700 text-slate-200">Play</button>
        <input value="100" class="self-end -translate-x-60 -translate-y-14 -rotate-90" @change="changeVolume"
            type="range" />
        <input @change="changeTimeAudio" value="0" ref="seek" :max="audioDuration" type="range" class="w-96" step="1" />
        <p>{{(seek && audioDuration) ? Math.floor(seek.value) + ' / ' + Math.ceil(audioDuration) : '0 / 0'}}
        </p>
        <SongsList :activeSrcIndex="activeSrcIndex"></SongsList>
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
const activeSrcIndex = ref({ index: 0 });

onMounted(() => {
    audio.value.src = '../assets/mp3/The Chemical Brothers - Galvanize.mp3';
})

watch(activeSrcIndex.value, () => {
    setSong(activeSrcIndex.value.index);
})

const changeVolume = (e) => {
    audio.value.volume = e.currentTarget.value / 100;
}

const changeSeek = () => {
    seek.value.value = Math.floor(audio.value.currentTime)
}

const changeTimeAudio = () => {
    audio.value.currentTime = seek.value.value
}

// Handle Pause/Play Button
const music = (e) => {
    if (!audio.value.src) {
        alert('Select Song');
        return;
    }
    isAudioPlay.value = !isAudioPlay.value
    if (isAudioPlay.value) {
        e.target.innerText = 'Pause';
        // audio.value.currentTime = audioTime.value;
        audioDuration.value = audio.value.duration;
        audio.value.play();
    }
    else if (!isAudioPlay.value) {
        e.target.innerText = 'Play';
        audio.value.pause();
    }
}

// Change selected song
const setSong = (index: number) => {
    if (isAudioPlay.value) {
        audio.value.pause();
        audio.value.src = '../assets/mp3/' + songList[index];
        audio.value.play();

    }
    else
        audio.value.src = '../assets/mp3/' + songList[index];

}

</script>