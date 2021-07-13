<template>
  <main class="container mx-auto p-4">
    <main class="grid gap-4 grid-cols-2 sm:grid-cols-4 lg:grid-cols-6">
      <article v-for="(category, categoryName) in categories" :key="categoryName + '-' + nanoid(8)">
        <h2 class="font-bold text-black dark:text-white">{{ categoryName }}</h2>
        <ul>
          <li v-for="(itemUrl, itemName) in category" :key="itemName + '-' + nanoid(8)">
            <a :href="itemUrl" class="text-black dark:text-white" :class="{ 'transition-colors text-blue-500 dark:text-blue-500 hover:text-red-500 inline-block': itemUrl !== null }">
              <span class="relative inline-block h-4 w-4" v-if="itemUrl !== null">
                <img :src="getFavicon(itemUrl)" class="relative inset-0 inline-block" onerror="this.style.display='none'" />
              </span>
              <span :class="{ underline: itemUrl !== null }">{{ itemName }}</span>
            </a>
          </li>
        </ul>
      </article>
    </main>
  </main>
</template>

<script>
import { nanoid } from 'nanoid';
export default {
  async asyncData({ $content }) {
    const data = (await $content().fetch())[0];
    return { ...data };
  },
  head() {
    return {
      title: this.title !== undefined ? this.title : 'Homepage',
    };
  },
  methods: {
    nanoid() {
      return nanoid(8);
    },
    getFavicon(url) {
      if (typeof url === 'string') {
        const match = url.match(/(?:https?:\/\/)?([\w.]*)/)[1];
        if (match.length > 0) {
          return 'https://' + match + '/favicon.ico';
        }
      }
      return null;
    },
  },
};
</script>

<style>
body {
  @apply bg-white dark:bg-black;
}
</style>
