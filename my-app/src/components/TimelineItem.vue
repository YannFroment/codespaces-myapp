<script setup lang="ts">
import { defineProps } from "vue";
import { TimelinePost } from "../interfaces/posts";

defineProps<{
  post: TimelinePost;
}>();

</script>

<template>
  <a>{{ post.title }}</a>
  <div>{{ post.created.toFormat("d MMM") }}</div>
</template>
