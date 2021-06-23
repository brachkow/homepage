<template>
  <main class="container mx-auto p-4">
    <main class="grid gap-4 grid-cols-2 sm:grid-cols-4 lg:grid-cols-6">
      <article v-for="(category, categoryName) in categories" :key="categoryName + '-' + nanoid(8)">
        <h2 class="font-bold">{{ categoryName }}</h2>
        <ul>
          <li v-for="(itemUrl, itemName) in category" :key="itemName + '-' + nanoid(8)">
            <a :href="itemUrl" :class="{ 'transition-colors text-blue-500 hover:text-red-500 inline-block': itemUrl !== null }">
              <span class="relative inline-block h-4 w-4">
                <img :src="getFavicon(itemUrl)" class="relative top-0 left-0 bottom-0 right-0 inline-block" onerror="this.style.display='none'" />
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
    log(value) {
      console.dir(value);
    },
  },
};
</script>
