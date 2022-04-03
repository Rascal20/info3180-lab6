
<template>
    <form @submit.prevent="searchNews" class="d-flex flex-column justify-content-center">
        <div class="input-group mx-sm-3 mb-2">
            <label class="visually-hidden" for="search">Search</label>
            <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
            <button class="btn btn-primary mb-2">Search</button>
        </div>
        <p>You are searching for {{ searchTerm }}</p>
    </form>


   
    <div class="news">
        <h2>News</h2>
        <ul class="news__list">
            <div class="row">
                <li v-for="article in articles" class="card">
                    <h5 class="card-body">{{ article.title }}</h5>
                    <img class="card-img-top" v-bind:src= "article.urlToImage">
                    <p class="card-body">{{ article.description }}</p>
                </li>
            </div>
        </ul>
    </div>

    
</template>
<script>
    export default {
        data() {
            return {
                articles: [],
                searchTerm: ''
            };
        },
        created() {
            let self = this;
            fetch(`https://newsapi.org/v2/top-headlines?country=us&apiKey=${import.meta.env.VITE_NEWSAPI_TOKEN}`,
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
        ,
        methods: {
            searchNews() {
                let self = this;
                fetch('https://newsapi.org/v2/everything?q='+ self.searchTerm + '&language=en', 
            {
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
        }
    };
</script>

<style>
body {
    padding-top: 5rem;
}

footer  {
    padding-top: 20px;
}

.card{
    width:20rem;
    margin:8px;
    display:inline-block;
    border-top: 5px solid teal;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2)
}

.news h2{
    text-align: center;
}
</style>