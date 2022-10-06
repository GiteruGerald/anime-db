<template>
  <div id="app">
    <header>
      <h1>My<strong>Anime</strong>app</h1>
      <form class="search-box" @submit.prevent="handleSearch">
        <input
          type="search"
          class="search-field"
          placeholder="Search for an anime..."
          required
          v-model="search_query"
        />
      </form>
    </header>
    <main>
      <div class="cards" v-if="animeList.length > 0">
        <Card v-for="anime in animeList" :key="anime.mal_id" :anime="anime" />
      </div>
      <div class="no-results" v-else>
        <h3>Sorry, we have no results...</h3>
      </div>
    </main>
  </div>
</template>
<script setup>
import Card from "./components/Card.vue";
import { onBeforeMount, ref } from "vue";
import axios from "axios";

const search_query = ref("");
const animeList = ref([]);

onBeforeMount(async ()=>{
  await axios.get(`https://api.jikan.moe/v4/anime`)
    .then((response)=>{
      animeList.value = response.data.data
    })
    .catch((err)=>{
      console.log(err)
    })

})

 
const handleSearch = async () => {
  await axios
    .get(`https://api.jikan.moe/v4/anime?q=${search_query.value}`)
    .then((response) => {
      animeList.value = response.data.data;

      search_query.value = ""
    })
    .catch((err) => {
      console.log(err);
    });
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
}
a {
  text-decoration: none;
}

header {
  padding-top: 50px;
  padding-bottom: 50px;

  h1 {
    color: #888;
    font-size: 42px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 20px;

    strong {
      color: #313131;
    }

    &:hover {
      color: #313131;
    }
  }

  .search-box {
    display: flex;
    justify-content: center;
    padding-left: 30px;
    padding-right: 30px;

    .search-field {
      appearance: none;
      background: none;
      border: none;
      outline: none;

      background-color: #f3f3f3;
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);

      display: block;
      width: 100%;
      max-width: 600px;
      padding: 15px;
      border-radius: 8px;

      color: #313131;
      font-size: 20px;

      transition: 0.4s;

      &::placeholder {
        color: #aaa;
      }

      &:focus,
      &:valid {
        color: #fff;
        background-color: #313131;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
      }
    }
  }
}
main {
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;

  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}
</style>
