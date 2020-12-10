<template>
  <div class="max-w-xs rounded overflow-hidden shadow-lg w-full h-full">
    <img
      class="w-full"
      :src="company.logoUrl || '/image-not-found.png'"
      onerror="this.onerror=null; this.src='/image-not-found.png';"
      alt="Sunset in the mountains"
    />
    <div class="px-6 py-4">
      <div class="font-bold text-xl mb-2">{{ title }}</div>
      <p class="text-grey-darker text-base">
        {{ company.name }}<br />
        {{ city }}
        {{ remote ? "Удаленно" : "В офисе" }}
      </p>
    </div>
    <div class="px-6 py-4">
      <span
        v-for="tag in tags"
        :key="tag.id"
        class="inline-block bg-grey-lighter rounded-full px-3 py-1 text-sm font-semibold text-grey-darker mr-2"
      >
        #{{ tag.name }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    company: Object,
    city: String,
    tags: Array,
    remote: Boolean,
  },
};
</script>

<style>
</style>