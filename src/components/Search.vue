<template>
    <div class="search">
        <input v-model="search" type="text" class="{{ isTyping ? 'typing' : null }}" placeholder="Search..." />
        <div v-if="isTyping" class="search-result">
            <div v-if="loading">
                <ContentLoader
                viewBox="0 0 500 60" v-for="i in searchResult.length" :key="i"
                >
                    <rect x="203" y="22" rx="0" ry="0" width="4" height="3" />
                    <rect x="15" y="10" rx="0" ry="0" width="71" height="40" />
                    <rect x="96" y="20" rx="0" ry="0" width="169" height="8" />
                    <rect x="96" y="35" rx="0" ry="0" width="92" height="6" />
                </ContentLoader>
            </div>
            <div v-else class="search-result-item" v-for="(aranan, index) in searchResult" :key="index">
                {{ aranan.name }}
            </div>
            <div v-if="searchResult.length <= 0 && loading === false" class="result-not-found">
                {{ search }} ile ilgili bir şey bulamadık.
            </div>
        </div>
    </div>
</template>

<script>
import {
  ContentLoader,
  FacebookLoader,
  CodeLoader,
  BulletListLoader,
  InstagramLoader,
  ListLoader,
} from 'vue-content-loader'

export default {
    name: "Search",
    components: {
        ContentLoader,
        FacebookLoader,
        ListLoader,
        CodeLoader,
        InstagramLoader,
        BulletListLoader
    },
    data(){
        return {
            search: "",
            result: [],
            loading: false,
            isTyping: ""
        }
    },
    computed: {
        isTyping(){
            this.loading = true;
            return this.search.replace(/\s+/, '').length > 0;
        },
        searchResult(){
            return this.result.filter((user) => {
                setTimeout(() => {
                    this.loading = false;
                }, 1000);
                return user.name.toLowerCase().match(this.search.toLowerCase());
            });
        }
    },
    async mounted() {
        const users = await fetch('https://jsonplaceholder.typicode.com/users')
            .then(response => response.json())
            .then(json => this.result = json);
    }
}    
</script>