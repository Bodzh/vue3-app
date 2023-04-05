<template>
    <nav class="is-primary panel">
      
        <span class="panel-tabs">
            <a v-for="(period, index) of periods" :key="index" @click="postsStore.setSelectedPeriod(period)"
                :class="{ 'is-active': postsStore.selectedPeriod === period }">
                {{ period }}
            </a>

        </span>
        <TimelineItem v-for="post of postsStore.filteredPosts" :key="post.id" :post="post" />
    </nav>
</template>

<script setup lang="ts">
import TimelineItem from '../components/TimeLineItem.vue';
import { usePosts } from '../stores/posts'
import { periods } from '../constants';

const postsStore = usePosts()
</script>

<style></style>