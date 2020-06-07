<template>
  <div id="app">
    <app-navbar></app-navbar>
    <app-movie-list></app-movie-list>
  </div>
</template>

<script>
import MovieList from './components/MovieList.vue';
import Navbar from './components/Navbar.vue';

export default {
  name: 'App',
  
  components: {
    appMovieList: MovieList,
    appNavbar: Navbar,
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    list-style: none;
    box-sizing: border-box;
    text-decoration: none;
  }
</style>
