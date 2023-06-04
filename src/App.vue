<script setup lang="ts">
// @ts-nocheck
import { ref } from 'vue'

const state = ref({
  steps: 0,
  selected: [],
  title: '',
  description: '',
  photos: [],
  maxWeight: 0
})

const btnState = ref({
  text: 'Pick',
  color: 'bg-blue-500 hover:bg-blue-700'
})

let formData = new FormData()

const handelNext = async () => {
  if (state.value.steps == 1) {
    // submit the form
    btnState.value = {
      text: 'Loading...',
      color: 'bg-blue-400 hover:bg-blue-400'
    }
    console.log(state.value)

    try {
      const res = await fetch(`${import.meta.env.VITE_SERVER_URL}/api/upload`, {
        method: 'POST',
        body: formData
      })
      const jsonData = await res.json()
      btnState.value = {
        text: 'Submitted',
        color: 'bg-blue-500 hover:bg-blue-700'
      }
      console.log({ jsonData })
    } catch (error) {
      btnState.value = {
        text: 'Error',
        color: 'bg-red-400 hover:bg-red-400'
      }
      console.log('Some err', error)
    }

    return
  }
  btnState.value = {
    text: 'Submit',
    color: 'bg-blue-500 hover:bg-blue-700'
  }
  state.value.steps += 1
}

const handelPrev = () => {
  btnState.value = {
    text: 'Pick',
    color: 'bg-blue-500 hover:bg-blue-700'
  }
  state.value.steps -= 1
}

const imageUploaded = (e: Event) => {
  for (let i = 0; i < (e.target as HTMLButtonElement).files.length; i++) {
    state.value.photos.push(URL.createObjectURL(e.target?.files[i]))
    formData.append('file', e.target!.files[i])
  }
}

const deleteImg = (img: string) => {
  state.value.photos = state.value.photos.filter((src) => src !== img)
}

const dropHandler = (event: DragEvent) => {
  console.log('Dropped', event)
  // Prevent default behavior (Prevent file from being opened)
  event.preventDefault()
  // Use DataTransfer interface to access the file(s)
  ;[...event.dataTransfer!.files].forEach((file, i) => {
    if (file.type.match('image')) {
      state.value.photos.push(URL.createObjectURL(file))
      formData.append('file', file)
    }
  })
}

const dragoverHandler = (event: DragEvent) => {
  // Prevent default behavior (Prevent file from being opened)
  event.preventDefault()
}
</script>

<template>
  <div class="min-h-screen py-10 flex flex-col justify-center items-center gap-y-4">
    <button
      v-if="state.steps > 0"
      @click="handelPrev"
      class="font-bold text-2xl px-4 rounded focus:outline-none focus:shadow-outline"
    >
      ⇦
    </button>

    <div v-if="state.steps === 0" class="w-56 grid grid-cols-2 justify-center gap-6 items-center">
      <label
        title="chest"
        for="chest"
        :class="[
          state.selected.includes('chest') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200 hover:text-black'
        ]"
      >
        <img src="./assets/chest.png" alt="chest" class="w-24 h-20" />
        <input class="sr-only" type="checkbox" id="chest" value="chest" v-model="state.selected" />
      </label>

      <label
        title="back"
        for="back"
        :class="[
          state.selected.includes('back') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200 hover:text-black'
        ]"
      >
        <img src="./assets/back.jpeg" alt="back" class="w-24 h-20" />
        <input class="sr-only" type="checkbox" id="back" value="back" v-model="state.selected" />
      </label>

      <label
        title="shoulder"
        for="shoulder"
        :class="[
          state.selected.includes('shoulder') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200 hover:text-black'
        ]"
      >
        <img src="./assets/shoulders.jpeg" alt="shoulder" class="w-24 h-20" />
        <input
          class="sr-only"
          type="checkbox"
          id="shoulder"
          value="shoulder"
          v-model="state.selected"
        />
      </label>

      <label
        title="legs"
        for="legs"
        :class="[
          state.selected.includes('legs') ? 'bg-slate-900 text-white' : '',
          'border border-black w-24 h-24 hover:bg-slate-200'
        ]"
      >
        <img src="./assets/legs.png" alt="legs" class="w-24 h-20" />
        <input class="sr-only" type="checkbox" id="legs" value="legs" v-model="state.selected" />
      </label>
    </div>
    <div v-else-if="state.steps === 1">
      <div class="w-full max-w-xs">
        <form class="bg-white shadow-md rounded px-8 pt-6 pb-8">
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="title"> Title </label>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="title"
              type="text"
              v-model="state.title"
              placeholder="Title"
            />
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="desc">
              Description
            </label>
            <textarea
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="desc"
              type="text"
              v-model="state.description"
              placeholder="Description"
            ></textarea>
          </div>
          <div class="mb-4">
            <label class="block text-gray-700 text-sm font-bold mb-2" for="weight">
              Max weight
            </label>
            <div class="relative">
              <span class="absolute top-2 right-2 text-gray-400">Kg</span>
              <input
                class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                id="weight"
                pattern="[0-9]*"
                type="text"
                v-model="state.maxWeight"
                inputmode="numeric"
                placeholder="30"
              />
            </div>
          </div>
          <div class="mb-4">
            <span class="block text-gray-700 text-sm font-bold mb-2"> Photos</span>
            <label
              for="photos"
              class="flex flex-col items-center justify-center w-full h-32 border-2 border-gray-300 border-dashed rounded-lg cursor-pointer bg-gray-50 dark:hover:bg-bray-800 dark:bg-gray-700 hover:bg-gray-100 dark:border-gray-600 dark:hover:border-gray-500 dark:hover:bg-gray-600"
            >
              <div
                class="flex flex-col items-center justify-center pt-5 pb-6"
                @drop="dropHandler($event)"
                @dragover="dragoverHandler($event)"
              >
                <svg
                  aria-hidden="true"
                  class="w-10 h-10 mb-3 text-gray-400"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"
                  ></path>
                </svg>
                <p class="mb-2 text-sm text-gray-500 dark:text-gray-400">
                  <span class="font-semibold">Click to upload</span> or drag and drop
                </p>
                <p class="text-xs text-gray-500 dark:text-gray-400">SVG, PNG, JPG</p>
              </div>
              <input
                id="photos"
                type="file"
                @change="(event) => imageUploaded(event)"
                multiple
                placeholder="Description"
                class="hidden"
              />
            </label>
          </div>

          <div v-if="state.photos.length > 0" class="mb-4">
            <div class="flex gap-2 flex-wrap justify-evenly">
              <div v-for="img in state.photos" :key="img" class="relative w-24 h-24">
                <button
                  type="button"
                  class="absolute top-0 right-1 font-lg text-lg w-6 h-6 bg-black flex justify-center items-center border border-black rounded-full text-white"
                  @click="deleteImg(img)"
                >
                  X
                </button>
                <img :src="img" :alt="img" class="h-24" />
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
    <button
      @click="handelNext"
      :disabled="state.selected.length === 0 || btnState.text === 'Submitted'"
      :class="[
        'text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline hover:bg-blue-200',
        state.selected.length === 0 ? 'bg-blue-200' : '',
        btnState.color
      ]"
    >
      {{ btnState.text }}
    </button>
  </div>
</template>

<style scoped>
input[type='number'] {
  -moz-appearance: textfield;
}
input[type='number']::-webkit-inner-spin-button,
input[type='number']::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
