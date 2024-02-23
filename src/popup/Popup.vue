<script setup lang="ts">
import { marked } from 'marked'
import { storageDemo } from '~/logic/storage'
import EpSuccessFilled from '~icons/ep/success-filled'

function openOptionsPage() {
  browser.runtime.openOptionsPage()
}

async function getCurrentTab() {
  const tabs = await browser.tabs.query({
    active: true,
    currentWindow: true,
  })

  return tabs[0]
}

const markdown = ref('')
const parsedMarkdown = ref('')

getCurrentTab()
  .then((tab) => {
    const { title, url } = tab
    markdown.value = `[${title}](${url})`
    return marked.parse(markdown.value)
  })
  .then(parsed => parsedMarkdown.value = parsed)
</script>

<template>
  <main class="px-4 py-5 text-center text-gray-700">
    <h1 class="text-6 mb-2">
      Successfully Copied
    </h1>
    <EpSuccessFilled class="text-green-700 text-4xl mx-auto mb-2" />

    <table>
      <tbody>
        <tr>
          <th class="border px-2 py-1">
            Content in Clipboard:
          </th>
          <td class="border px-2 py-1">
            {{ markdown }}
          </td>
        </tr>
        <tr>
          <th class="border px-2 py-1">
            After Render:
          </th>
          <td class="border px-2 py-1" v-html="parsedMarkdown" />
        </tr>
      </tbody>
    </table>

    <button class="btn mt-2" @click="openOptionsPage">
      Open Options
    </button>
    <div class="mt-2">
      <span class="opacity-50">Storage:</span> {{ storageDemo }}
    </div>
  </main>
</template>
