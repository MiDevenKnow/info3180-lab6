<template>
    <form @submit.prevent="searchNews" class="d-flex flex-column justify-content-center mb-3">
        <div class="input-group mb-2">
            <label class="visually-hidden" for="search">Search</label>
            <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
            <button class="btn btn-primary mb-2">Search</button>
        </div>
        <p> <span class="fw-bold">You are searching for:</span> {{ searchTerm }}</p>
    </form>
    <div class="row row-cols-1 row-cols-md-3 g-4">
        <div v-for="article in articles" :key="article" class="col">
            <div class="card h-100">
                <img :src="article.urlToImage" class="card-img-top" alt="...">
                <div class="card-body">
                    <h5 class="card-title">{{ article.title }}</h5>
                    <p class="card-text">{{ article.description }}</p>
                </div>
                <div class="card-footer bg-success" height="1%">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
         return {
            articles: [],
            searchTerm: ''
        }
    },
     methods: {
        searchNews() {
        let self = this;
        fetch('https://newsapi.org/v2/everything?q='+self.searchTerm + '&language=en', {
            headers: {
            'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
            }
        })
        .then(function(response) {
            return response.json();
        })
        .then(function(data) {
            console.log(data);
            self.articles = data.articles;
        });
        }
    },
    created() {
        let self = this;
        fetch('https://newsapi.org/v2/top-headlines?country=us',
        {
            headers: {
            'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
            }
        })
        .then(function(response) {
            return response.json();
        })

        .then(function(data) {
            console.log(data);
            self.articles = data.articles;
        });
    }
};
</script>