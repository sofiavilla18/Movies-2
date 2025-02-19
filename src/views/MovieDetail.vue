<template>
  <div class="movie-detail">
    <h2>{{ movie.Title }}</h2>
    <p>{{ movie.Year }}</p>
    <img :src="movie.Poster" alt="movie Poster" class="featured-imgen"/>
    <p>{{ movie.Plot }}</p>
  </div>
</template>

<script>
import { ref, onBeforeMount } from 'vue';
import { useRoute } from 'vue-router';
import env from '@/env.js';

export default {
  setup() {
    const movie = ref({});
    const route = useRoute();

    onBeforeMount(() => {
      fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
        .then(response => response.json())
        .then(data => {
          movie.value = data;
        });
    });

    return {
      movie
    };
  }
};
</script>

<style>
   .movie-detail {
   padding: 16px;
   }

   .movie-detail h2 {
      color: #fff;
      font-size: 28px;
      font-weight: 600;
   }

   .featured-imgen {
      /*la imagen sera un bloque*/
      display: block;
      max-width: 200px;
      margin-bottom: 16px;
   }

   p{
      color: #fff;
      font-size: 18px;
      line-height: 1.5;
   }

</style>

