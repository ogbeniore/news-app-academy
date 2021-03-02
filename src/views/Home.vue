<template>
<div>
  <h1>Top Headlines</h1>
  <br>
  <nav class="navigation">
    <ul class="navigation__menu">
      <li class="navigation__item" v-for="category in categories" :key="category" @click="fetchNewsByCategory(category)">
        {{ category }}
      </li>
    </ul>
  </nav>
  <div class="home">
    <h1 v-if="isLoading">Loading...</h1>
    <div class="posts" v-else>
      <div class="post__single" v-for="headline in allHeadlines" :key="headline.id">
        <div class="post__image">
          <img :src="headline.urlToImage" :alt="headline.title">
        </div>
        <h3>{{ headline.title }}</h3>
        <p v-html="headline.content"></p>
        <small>source: {{ headline.source.name }}</small>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from "axios";

export default {
  name: 'Home',
  data() {
    return {
      allHeadlines: [],
      isLoading: true,
      apiKey: '37e5489fb84641b7918926592dad382c',
      apiUrl: 'https://newsapi.org/v2/top-headlines',
      categories: ['all', 'business', 'entertainment', 'general', 'health', 'science', 'sports', 'technology']
    }
  },
  mounted() {
    const { apiUrl, apiKey } = this
    axios
      .get(`${apiUrl}?country=ng&apiKey=${apiKey}`)
      .then(({ data }) => {
        const { articles } = data
        this.allHeadlines = articles
      })
      .catch(error => {
        console.log(error)
      })
      .finally(() => { this.isLoading = false })
  },
  methods: {
    fetchNewsByCategory(cat) {
      const { apiUrl, apiKey } = this
      this.isLoading = true
      axios
      .get(`${apiUrl}?country=ng&apiKey=${apiKey}&category=${cat !== 'all' ? cat : ''}`)
      .then(({ data }) => {
        const { articles } = data
        this.allHeadlines = articles
      })
      .catch(error => {
        console.log(error)
      })
      .finally(() => { this.isLoading = false })
    }
  }
}
</script>

<style scoped>
.home {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  background: #ececec;
  min-height: 70vh;
  padding: 30px;
}
.posts {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
}
.post__single {
  border: 1px solid #333333;
  border-radius: 4px;
  padding: 16px;
  overflow: hidden;
}
.post__image {
  width: 100%;
  height: 200px;
  overflow: hidden;
}
img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  overflow: hidden;
}
.navigation {
  width: 100%;
  max-width: 1400px;
  margin: 0 auto;
  padding: 20px;
  border-top: 2px solid black;
  border-bottom: 2px solid black;
}
.navigation__menu {
  list-style: none;
  display: flex;
  justify-content: space-between;
}
.navigation__menu li {
  padding: 0 10px;
  text-transform: uppercase;
  font-size: 18px;
  font-weight: 600;
  cursor: pointer;
}
</style>