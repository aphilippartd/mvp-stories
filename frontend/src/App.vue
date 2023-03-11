<template>
  <h1 class="text-center text-2xl my-8">Generate me a story</h1>
  <div class="w-screen flex items-center justify-center flex-col">
    <div class="flex items-center justify-center flex-col">
      <template v-if="!loading">
        <div class="relative mt-4 flex items-center w-96">
          <input v-model="query" @keyup.enter="onPressEnter" type="text" name="search" id="search" placeholder="Context of this story" class="block w-full rounded-md border-gray-300 pr-12 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" />
          <div class="absolute inset-y-0 right-0 flex py-1.5 pr-1.5 cursor-pointer" @click="onPressEnter">
            <kbd class="inline-flex items-center rounded border border-gray-200 px-2 font-sans text-sm font-medium text-gray-400">🔍</kbd>
          </div>
        </div>
        <div class="mt-8">
          <p class="text-sm leading-5 text-gray-500">Language of story?</p>
          <fieldset class="mt-4">
            <div class="space-y-4 sm:flex sm:items-center sm:space-y-0 sm:space-x-10">
              <div v-for="loc in locales" :key="loc.id" class="flex items-center">
                <input :id="loc.id" name="notification-method" type="radio" :checked="loc.id === locale" @input="_ => locale = loc.id" class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-500" />
                <label :for="loc.id" class="ml-3 block text-sm font-medium text-gray-700">{{ loc.title }}</label>
              </div>
            </div>
          </fieldset>
        </div>
      </template>
      <div class="text-center text-xl text-blue-400" v-else>Generating magical story ...</div>
    </div>
    <div v-if="mp3Url" class="mt-8 text-center text-xl">
      <h2>Your story 👇, enjoy!</h2>
      <audio controls>
        <source :src="mp3Url" type="audio/mpeg">
      </audio>
    </div>    
  </div>
</template>


<script setup>
import { ref } from 'vue'

const query = ref('')
const loading = ref(false)
const mp3Url = ref('')
const locale = ref('en')
const locales = [
  { id: 'en', title: 'English 🇬🇧' },
  { id: 'fr', title: 'French 🇫🇷' },
  { id: 'ar', title: 'Arabic 🇪🇬' },
  { id: 'it', title: 'Italian 🇮🇹' },
  { id: 'nl', title: 'Dutch 🇳🇱' },
]

const onPressEnter = async () => {
  if (!query.value) return
  if (loading.value) return
  mp3Url.value = ''
  loading.value = true
  const response = await fetch("YOUR_API_ENDPOINT", {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      context: query.value,
      locale: locale.value
    })
  })
  const responseJson = await response.json()
  mp3Url.value = responseJson.mp3Url
  loading.value = false
  query.value = ''
}

</script>