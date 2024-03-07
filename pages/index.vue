<template>
    <div class="grid grid-cols-4">
        <div><UButton color="gray" variant="solid" to="/read">Already Read</UButton></div>
        <div class="col-start-4 mb-4">
            <UInput
                v-model="searchQuery"
                @input="search"
                icon="i-heroicons-magnifying-glass-20-solid"
                size="sm"
                color="white"
                :trailing="false"
                placeholder="Search..."
            />
        </div>
    </div>
    <div v-if="pending">
        <div class="grid grid-cols-4 gap-4">
            <UCard>
                <template #header>
                    <USkeleton class="h-[150px] w-[250px]" />
                </template>

                <div>
                    <USkeleton class="h-4 w-[250px] mb-2" />
                    <USkeleton class="h-10 w-[250px]" />
                </div>

                <template #footer>
                    <USkeleton class="h-4 w-[200px] mb-1" />
                    <USkeleton class="h-4 w-[200px] mb-1" />
                    <USkeleton class="h-4 w-[200px]" />
                </template>
            </UCard>
            <UCard>
                <template #header>
                    <USkeleton class="h-[150px] w-[250px]" />
                </template>

                <div>
                    <USkeleton class="h-4 w-[250px] mb-2" />
                    <USkeleton class="h-10 w-[250px]" />
                </div>

                <template #footer>
                    <USkeleton class="h-4 w-[200px] mb-1" />
                    <USkeleton class="h-4 w-[200px] mb-1" />
                    <USkeleton class="h-4 w-[200px]" />
                </template>
            </UCard>
            <UCard>
                <template #header>
                    <USkeleton class="h-[150px] w-[250px]" />
                </template>

                <div>
                    <USkeleton class="h-4 w-[250px] mb-2" />
                    <USkeleton class="h-10 w-[250px]" />
                </div>

                <template #footer>
                    <USkeleton class="h-4 w-[200px] mb-1" />
                    <USkeleton class="h-4 w-[200px] mb-1" />
                    <USkeleton class="h-4 w-[200px]" />
                </template>
            </UCard>
        </div>
    </div>
    <div v-else>
        <div v-if="posts.status === 'ok'" class="grid grid-cols-4 gap-4">
            <UCard v-for="(post, index) in filteredArticles" :class="{ 'col-span-2 row-span-2': index === 0 || index === 7}">
                <template #header>
                    <div v-if="post?.urlToImage">
                        <a @click.prevent="saveToLocalStorage(post)" :href="post.url" :title="post.title"><img :src="post.urlToImage" :alt="post.title"></a>
                    </div>
                    <div v-else><USkeleton class="h-[150px] w-[250px]" /></div>
                </template>

                <div>
                    <h2 class="text-lg font-bold" v-if="post?.url">
                        <a @click.prevent="saveToLocalStorage(post)" :href="post.url" :title="post.title">{{ post.title }}</a>
                    </h2>
                    <h2 v-else><USkeleton class="h-4 w-[250px] mb-2" /></h2>
                    <p class="text-gray-400" v-if="post?.description">{{ post.description }}</p>
                    <p v-else><USkeleton class="h-10 w-[250px]" /></p>
                </div>

                <template #footer>
                    <div v-if="post?.author"><UIcon name="i-heroicons-user" /> {{ isFacebook(post.author) ? isFacebook(post.author)?.pathname.slice(1) : post.author }}</div><div v-else><USkeleton class="h-4 w-[200px] mb-1" /></div>
                    <div v-if="post?.publishedAt"><UIcon name="i-heroicons-calendar-days" /> {{ formatDate(post.publishedAt) }}</div><div v-else><USkeleton class="h-4 w-[200px] mb-1" /></div>
                    <div v-if="post?.source?.name"><UIcon name="i-heroicons-rss" /> {{ post.source.name }}</div><div v-else><USkeleton class="h-4 w-[200px]" /></div>
                </template>
            </UCard>
        </div>
        <div v-else>
            Data not found
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
const conf = useRuntimeConfig()
const searchQuery = ref('');
const { pending, data: posts } = await useFetch(conf.public.apiBase + '/v2/everything?q=indonesia&pageSize=10&apiKey=' + conf.public.apiKey, {
    lazy: true
})

const articles = ref(posts?.value?.articles || []);
const filteredArticles = ref(posts.value);

const search = () => {
    if (searchQuery.value.trim() === '') {
        filteredArticles.value = articles.value;
    } else {
        filteredArticles.value = articles.value.filter(article =>
            article.title.toLowerCase().includes(searchQuery.value.toLowerCase())
        );
    }
};

onMounted(() => {
  search();
});

const saveToLocalStorage = (post) => {
    const readNews = {
        title: post.title,
        url: post.url,
        image: post.urlToImage,
    };
    const existingNews = JSON.parse(localStorage.getItem('readNews')) || [];
    const updatedItems = [...existingNews, readNews];
    const isAlreadySaved = existingNews.some(news => news.title === readNews.title);
    if (!isAlreadySaved) localStorage.setItem('readNews', JSON.stringify(updatedItems));
    window.open(post.url);
};

function isFacebook(string: string) {
    if (string && string.toLowerCase().includes("facebook.com")) {
        return new URL(string);
    } else {
        return false
    }
}

function formatDate(string: string) {
    const date = new Date(string);
    const tahun = date.getFullYear();
    const bulan = date.getMonth();
    const tanggal = date.getDate();
    const hari = date.getDay();
    const jam = date.getHours().toString().padStart(2, '0');
    const menit = date.getMinutes().toString().padStart(2, '0');
    const detik = date.getSeconds();
    let hariStr = ''
    let bulanStr = ''
    switch(hari) {
        case 0:
            hariStr = "Minggu";
            break;
        case 1: 
            hariStr = "Senin";
            break;
        case 2: 
            hariStr = "Selasa";
            break;
        case 3: 
            hariStr = "Rabu";
            break;
        case 4: 
            hariStr = "Kamis";
            break;
        case 5: 
            hariStr = "Jumat";
            break;
        case 6: 
            hariStr = "Sabtu";
            break;
    }
    switch(bulan) {
        case 0:
            bulanStr = "Januari";
            break;
        case 1:
            bulanStr = "Februari";
            break;
        case 2:
            bulanStr = "Maret";
            break;
        case 3:
            bulanStr = "April";
            break;
        case 4:
            bulanStr = "Mei";
            break;
        case 5:
            bulanStr = "Juni";
            break;
        case 6:
            bulanStr = "Juli";
            break;
        case 7:
            bulanStr = "Agustus";
            break;
        case 8:
            bulanStr = "September";
            break;
        case 9:
            bulanStr = "Oktober";
            break;
        case 10:
            bulanStr = "November";
            break;
        case 11:
            bulanStr = "Desember";
            break;
    }

    return `${hariStr}, ${tanggal} ${bulanStr} ${jam}.${menit}`
}
</script>