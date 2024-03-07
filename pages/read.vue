<template>
    <div>
        <h1 class="font-bold text-xl mb-4">Already Read</h1>
        <div v-if="readNews?.length > 0" class="grid grid-cols-4 gap-4">
            <UCard v-for="(post, index) in readNews">
                <template #header>
                    <div v-if="post?.image">
                        <a :href="post.url" :title="post.title"><img :src="post.image" :alt="post.title"></a>
                    </div>
                    <div v-else><USkeleton class="h-[150px] w-[250px]" /></div>
                </template>
                <div>
                    <h2 class="text-lg font-bold" v-if="post?.url">
                        <a :href="post.url" :title="post.title">{{ post.title }}</a>
                    </h2>
                    <h2 v-else><USkeleton class="h-4 w-[250px] mb-2" /></h2>
                </div>
            </UCard>
        </div>
        <p v-else>Data not found.</p>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const readNews = ref(getReadNews());

function getReadNews() {
    if (typeof localStorage !== 'undefined') {
        const storedNews = localStorage.getItem('readNews');
        return storedNews ? JSON.parse(storedNews) : null;
    }
}
</script>