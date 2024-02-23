<script setup lang="ts">
import { marked } from 'marked'
import { storageDemo } from '~/logic/storage'
import EpSuccessFilled from '~icons/ep/success-filled'

function openOptionsPage() {
  browser.runtime.openOptionsPage()
}

const htmlOfCopied = ref('')

browser.tabs.query({
  active: true,
  currentWindow: true,
}).then(tabs => marked.parse(`[${tabs[0].title}](${tabs[0].url})`))
  .then(html => htmlOfCopied.value = html)
</script>

<template>
  <main class="w-[300px] px-4 py-5 text-center text-gray-700">
    <EpSuccessFilled class="text-green-700 text-4xl mx-auto" />
    <SharedSubtitle />

    <div v-html="htmlOfCopied" />

    <button class="btn mt-2" @click="openOptionsPage">
      Open Options
    </button>
    <div class="mt-2">
      <span class="opacity-50">Storage:</span> {{ storageDemo }}
    </div>
  </main>
</template>
