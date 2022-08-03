<template>
  <ul class="flex justify-center items-center">
    <li>
      <button
        type="button"
        class="
          rounded-full
          w-10
          h-10
          p-1
          text-base
          m-1
          hover:bg-gray-200
          text-center
        "
        :class="isInFirstPage ? 'pointer-events-none' : ''"
        @click="onClickFirstPage"
        :disabled="isInFirstPage"
      >
        <img font-bold src="../assets/double-arrow-left.svg" />
      </button>
    </li>
    <li>
      <button
        type="button"
        class="rounded-full w-10 h-10 p-1 text-base m-1 hover:bg-gray-200"
        :class="isInFirstPage ? 'pointer-events-none' : ''"
        @click="onClickPreviousPage"
        :disabled="isInFirstPage"
      >
        <img font-bold src="../assets/arrow-left.svg" />
      </button>
    </li>
    <li
      v-for="page in pages"
      :key="page.name"
      @click="onClickPage(page.name)"
      class="cursor-pointer"
    >
      <button
        class="rounded-full w-10 h-10 p-1 text-base m-1 hover:bg-gray-200"
        :class="isPageActive(page.name) ? 'bg-blue-100  ' : ''"
        type="button"
        :disabled="page.isDisabled"
      >
        {{ page.name }}
      </button>
    </li>
    <li>
      <button
        type="button"
        class="rounded-full w-10 h-10 p-1 text-base m-1 hover:bg-gray-200"
        :class="isInLastPage ? 'pointer-events-none' : ''"
        @click="onClickNextPage"
        :disabled="isInLastPage"
      >
        <img font-bold src="../assets/arrow-right.svg" />
      </button>
    </li>

    <li>
      <button
        type="button"
        class="rounded-full w-10 h-10 p-1 text-base m-1 hover:bg-gray-200"
        :class="isInLastPage ? 'pointer-events-none' : ''"
        @click="onClickLastPage"
        :disabled="isInLastPage"
      >
        <img font-bold src="../assets/double-arrow-right.svg" />
      </button>
    </li>
  </ul>
</template>

<script setup>
const currentPage = ref(1);
import { defineProps, computed, ref } from "vue";
const props = defineProps({
  maxVisibleButtons: {
    type: Number,
    required: false,
    default: 5,
  },
  totalPages: { type: Number, required: true },
  perPage: { type: Number, required: true },
});

const startPage = computed(() => {
  if (currentPage.value === 1) {
    return 1;
  }
  if (currentPage.value === props.totalPages) {
    const start = props.totalPages - (props.maxVisibleButtons - 1);

    if (start === 0) {
      return 1;
    } else {
      return start;
    }
  }
  if (currentPage.value === 2) {
    return currentPage.value - 1;
  }
  if (currentPage.value === 9) {
    return currentPage.value - 3;
  }
  return currentPage.value - 2;
});

const pages = computed(() => {
  const range = [];

  for (
    let i = startPage.value;
    i <=
    Math.min(startPage.value + props.maxVisibleButtons - 1, props.totalPages);
    i++
  ) {
    range.push({ name: i, isDisabled: i === currentPage.value });
  }
  return range;
});

const isInFirstPage = computed(() => {
  return currentPage.value === 1;
});

const isInLastPage = computed(() => {
  return currentPage.value === props.totalPages;
});

const onClickFirstPage = () => {
  currentPage.value = 1;
};
const onClickPreviousPage = () => {
  currentPage.value = currentPage.value - 1;
};
const onClickPage = (page) => {
  currentPage.value = page;
};
const onClickNextPage = () => {
  currentPage.value = currentPage.value + 1;
};

const onClickLastPage = () => {
  currentPage.value = props.totalPages;
};

const isPageActive = (page) => {
  return currentPage.value === page;
};
</script>

<style>
</style>

