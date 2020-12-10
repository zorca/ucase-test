<template>
  <div class="relative">
    <input
      :value="value"
      @input="handleInput"
      placeholder="Искасть вакансии в городе:"
      ref="input"
      tabindex="0"
      class="border border-gray-300 py-2 px-3 rounded-md focus:outline-none focus:shadow-md w-full"
    />
    <span
      v-if="value"
      @click.prevent="reset()"
      class="absolute inset-y-0 right-0 pr-3 flex items-center cursor-pointer"
    >
      x
    </span>
    <div
      v-show="value && showOptions"
      @focusout="showOptions = false"
      tabindex="0"
      class="absolute w-full z-50 bg-white border border-gray-300 mt-1 mh-48 overflow-hidden overflow-y-scroll rounded-md shadow-md"
    >
      <ul class="py-1">
        <li
          v-for="(item, index) in searchResults"
          :key="index"
          @click="handleClick(item)"
          class="px-3 py-2 cursor-pointer hover:bg-gray-200"
        >
          {{ item.name }}
        </li>
        <li v-if="!searchResults.length" class="px-3 py-2 text-center">
          Не удалось найти :(
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      required: false,
    },
    data: {
      type: Array,
      required: true,
    },
  },

  data() {
    return {
      showOptions: false,
      chosenOption: "",
      searchTerm: "",
    };
  },

  computed: {
    searchResults() {
      return this.data.filter((item) => {
        return item.name.toLowerCase().includes(this.searchTerm.toLowerCase());
      });
    },
  },

  methods: {
    reset() {
      this.$emit("input", "");
      this.chosenOption = "";
    },

    handleInput(evt) {
      this.$emit("input", evt.target.value);
      this.searchTerm = evt.target.value;
      this.showOptions = true;
    },

    handleClick(item) {
      this.$emit("input", item.name);
      this.$emit("chosen", item);
      this.chosenOption = item.name;
      this.showOptions = false;
      this.$refs.input.focus();
    },
  },
};
</script>

<style scoped>
.mh-48 {
  max-height: 10rem;
}
</style>