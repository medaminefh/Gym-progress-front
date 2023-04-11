<script setup lang="ts">
import { ref } from 'vue'

const selected = ref([])
const shoForm = ref(false)
const state = ref({
  title: '',
  description: '',
  photos: [],
  maxWeight: 0
})
</script>

<template>
  <div class="h-screen flex justify-center items-center">
    <div class="w-56 grid grid-cols-1 md:grid-cols-2 justify-center gap-6 items-center">
      <label
        title="chest"
        for="chest"
        :class="[
          selected.includes('chest') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200 hover:text-black'
        ]"
      >
        <img src="./assets/chest.png" alt="chest" class="w-24 h-20" />
        <input class="sr-only" type="checkbox" id="chest" value="chest" v-model="selected" />
      </label>

      <label
        title="back"
        for="back"
        :class="[
          selected.includes('back') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200 hover:text-black'
        ]"
      >
        <img src="./assets/back.jpeg" alt="back" class="w-24 h-20" />
        <input class="sr-only" type="checkbox" id="back" value="back" v-model="selected" />
      </label>

      <label
        title="shoulder"
        for="shoulder"
        :class="[
          selected.includes('shoulder') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200 hover:text-black'
        ]"
      >
        <img src="./assets/shoulders.jpeg" alt="shoulder" class="w-24 h-20" />
        <input class="sr-only" type="checkbox" id="shoulder" value="shoulder" v-model="selected" />
      </label>

      <label
        title="legs"
        for="legs"
        :class="[
          selected.includes('legs') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200'
        ]"
      >
        <img src="./assets/legs.png" alt="legs" class="w-24 h-20" />
        <input class="sr-only" type="checkbox" id="legs" value="legs" v-model="selected" />
      </label>
      <button class="border-solid border-2 border-indigo-600 rounded-full w-full">Pick</button>
    </div>
  </div>
</template>
