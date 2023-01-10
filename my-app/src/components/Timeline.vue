<script setup lang="ts">
import { ref, computed } from "vue";
import { DateTime } from "luxon";
import { periods, Period } from "../constants/periods";
import { TimelinePost, today, thisWeek, thisMonth } from "../interfaces/posts";
import TimelineItem from "./TimelineItem.vue";

const posts = computed<TimelinePost[]>(() => {
  return [today, thisWeek, thisMonth]
    .map((post) => {
      return {
        ...post,
        created: DateTime.fromISO(post.created),
      };
    })
    .filter((post) => {
      if (selectedPeriod.value === "Today") {
        return post.created >= DateTime.now().minus({ days: 1 });
      }
      if (selectedPeriod.value === "This Week") {
        return post.created >= DateTime.now().minus({ week: 1 });
      }
      return post;
    });
});

const selectedPeriod = ref<Period>("Today");

function setSelectedPeriod(period: Period) {
  selectedPeriod.value = period;
}
</script>

<template>
  {{ selectedPeriod }}
  <nav class="is-primary panel">
    <span
      v-for="period of periods"
      :key="period"
      class="panel-tabs"
      @click="setSelectedPeriod(period)"
    >
      <a>{{ period }}</a>
    </span>
  </nav>

  <TimelineItem
    v-for="post of posts"
    :key="post.id"
    :post="post"
    class="panel-block"
  />
</template>
