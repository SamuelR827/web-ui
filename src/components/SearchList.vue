<template>
  <div class="min-h-screen m-2">
    <div v-if="sm.isSearching.value || props.error" class="hero min-h-screen">
      <button v-if="sm.isSearching" class="btn btn-sm btn-ghost loading">
        LOADING
      </button>
      <div v-if="props.error" class="alert alert-error">
        <div class="flex-1">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            class="w-6 h-6 mx-2 stroke-current"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M18.364 18.364A9 9 0 005.636 5.636m12.728 12.728A9 9 0 015.636 5.636m12.728 12.728L5.636 5.636"
            ></path>
          </svg>
          <label>Error: {{ sm.errorValue }}</label>
        </div>
      </div>
    </div>
    <div
      v-else
      v-for="(song, index) in props.data.slice(0, 15)"
      :key="index"
      class="card md:card-side card-bordered my-2 shadow-lg card-compact bg-base-100"
    >
      <!-- {{ song }} -->
      <figure class="aspect-square md:max-h-fit">
        <img
          :src="song.cover_url"
          class="object-contain aspect-square md:max-h-44"
        />
      </figure>
      <div class="card-body">
        <h2 class="card-title">
          {{ song.name }}
          <div class="badge mx-0.5 badge-error" v-if="song.explicit">
            Explicit
          </div>
        </h2>
        <h3>
          <a v-for="(artist, index) in song.artists" :key="index">
            <a v-if="index !== 0"> &#8226; </a>
            {{ artist }}
          </a>
        </h3>
        <h3>
          {{ song.album_name }}
        </h3>
        <br />

        <p>
          <br />
        </p>
        <div class="card-actions absolute bottom-0 right-0 m-2">
          <a class="btn btn-ghost btn-square" :href="song.url" target="_blank">
            <!-- Spotify -->

            <Icon icon="clarity:link-line" class="h-6 w-6" />
          </a>

          <button
            v-if="pt.getBySong(song)?.isQueued()"
            class="btn btn-primary btn-square"
          >
            <!-- in queue -->

            <Icon icon="clarity:check-line" class="h-6 w-6" />
          </button>
          <button
            v-else
            class="btn btn-primary btn-square"
            @click="download(song)"
          >
            <!-- download -->

            <!-- <Icon icon="clarity:download-line" class="h-6 w-6" /> -->
            <!-- <Icon icon="clarity:clock-line" class="h-6 w-6" /> -->
            <!-- <Icon icon="clarity:upload-cloud-line" class="h-6 w-6" /> -->
            <Icon icon="clarity:floppy-line" class="h-6 w-6" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue'

import { useSearchManager } from '../model/search'
import { useProgressTracker, useDownloadManager } from '../model/download'

const props = defineProps(['data', 'error'])
console.log('props', props)

const emit = defineEmits(['download'])

const sm = useSearchManager()
const pt = useProgressTracker()
const dm = useDownloadManager()

function download(song) {
  emit('download', song)
}
</script>

<style scoped></style>
