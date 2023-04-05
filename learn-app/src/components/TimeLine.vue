<template>
    <nav class="is-primary panel">
        {{ postsStore.foo }}
        <button @click="postsStore.updateFoo('bar')"></button>
        <span class="panel-tabs">
            <a v-for="(period, index) of periods" :key="index" @click="selectPeriod(period)"
                :class="{ 'is-active': selectedPeriod === period }">
                {{ period }}
            </a>

        </span>
    <TimelineItem v-for="post of posts" :key="post.id" :post="post"/>
    </nav>
</template>

<script setup lang="ts">
import { DateTime } from 'luxon'
import { ref, computed } from 'vue'
import { today, thisMonth, thisWeek, TimelinePost } from '../posts'
import  TimelineItem  from '../components/TimeLineItem.vue';
import {usePosts} from '../stores/posts'
// type Period = 'Today' | 'This week' | 'This month'
const periods = ['Today', 'This week', 'This month'] as const
type Period = typeof periods[number]
const postsStore = usePosts()

let selectedPeriod = ref<Period>('Today')

function selectPeriod(period: Period) {
    selectedPeriod.value = period
}
const posts = computed<TimelinePost[]>(() => {
    return [
        today,
        thisWeek,
        thisMonth
    ].map(post => ({ ...post, created: DateTime.fromISO(post.created) }))
        .filter(post => {
            if (selectedPeriod.value === 'Today') {
                return post.created >= DateTime.now().minus({ day: 1 })
            }

            if (selectedPeriod.value === 'This week') {
                return post.created >= DateTime.now().minus({ week: 1 })
            }

            return post
        })
})
</script>

<style></style>